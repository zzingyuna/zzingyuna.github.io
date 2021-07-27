---
layout: post
---

# Selenium WebDriver를 이용한 크롬창 띄우기

#### 크롬창으로 google 로그인 후 photo가져오기

```
using System;
using System.Collections.Generic;
using System.Threading;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;

namespace ConsoleApp1
{
	class Program
	{
		static void Main(string[] args)
		{
			using (IWebDriver driver = new ChromeDriver())
			{
				driver.Url = "https://accounts.google.com/o/oauth2/v2/auth?scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fphotoslibrary%20https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fphotoslibrary.readonly&access_type=offline&include_granted_scopes=true&state=state_parameter_passthrough_value&redirect_uri=http%3A%2F%2Flocalhost%2Fstudy%2Ftest.html&response_type=code&client_id={client_id}";

				IWebElement txtmail = driver.FindElement(By.Id("identifierId"));//driver.FindElement(By.Name("q"));
				txtmail.SendKeys("kimyuna9944@gmail.com");

				IWebElement btnNext = driver.FindElement(By.Id("identifierNext"));
				btnNext.Click();

				Thread.Sleep(1000);

				IWebElement txtpw = driver.FindElement(By.Name("password"));
				txtpw.SendKeys("비번");

				IWebElement btnLogin = driver.FindElement(By.Id("passwordNext"));
				btnLogin.Click();

				Thread.Sleep(6000);

				IWebElement getPhotos = driver.FindElement(By.Id("linkPhoto"));
				getPhotos.Click();

				Thread.Sleep(7000);

				ICollection<IWebElement> imgPhotos = driver.FindElements(By.ClassName("googlePhotos"));//driver.FindElement(By.Name("q"));
				Console.WriteLine(imgPhotos.Count);
				foreach (IWebElement item in imgPhotos)
				{
					Console.WriteLine(item.GetAttribute("src"));
				}
			}

		}
		
	}
}

```
아래 드라이버 Nuget 설치할것! (OpenQA.Selenium 사용을 위해 필요)  
![alt text](https://zzingyuna.github.io/image/Selenium_WebDriver.JPG)  
  
  


#### 크롬창으로 google 로그인 후 photo가져오기 _ version2  
"고급" > 페이지로 이동 추가

```
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Threading;


namespace GooglePhotos
{
	class Program
	{
		[STAThread]
		static void Main(string[] args)
		{
			try
			{
				string redirect_url = ConfigurationManager.AppSettings["REDIRECTURL"].ToString();
				string scope = ConfigurationManager.AppSettings["SCOPE"].ToString();
				string access_type = ConfigurationManager.AppSettings["ACCESSTYPE"].ToString();
				string include_granted_scopes = ConfigurationManager.AppSettings["INCLUDEGRANTEDSCOPES"].ToString();
				string state = ConfigurationManager.AppSettings["STATE"].ToString();
				string response_type = ConfigurationManager.AppSettings["RESPONSETYPE"].ToString();
				string client_id = ConfigurationManager.AppSettings["CLIENTID"].ToString();
				string loginid = ConfigurationManager.AppSettings["LOGINID"].ToString();
				string password = ConfigurationManager.AppSettings["PWD"].ToString();

				using (IWebDriver driver = new ChromeDriver(Environment.CurrentDirectory))
				{
					try
					{
						driver.Url = "https://accounts.google.com/o/oauth2/v2/auth?scope=" + scope
							+ "&access_type=" + access_type
							+ "&include_granted_scopes=" + include_granted_scopes
							+ "&state=" + state
							+ "&redirect_uri=" + redirect_url
							+ "&response_type=" + response_type
							+ "&client_id=" + client_id;

						IWebElement txtmail = driver.FindElement(By.Id("identifierId"));
						txtmail.SendKeys(loginid);

						IWebElement btnNext = driver.FindElement(By.Id("identifierNext"));
						btnNext.Click();

						Thread.Sleep(1000);

						IWebElement txtpw = driver.FindElement(By.Name("password"));
						txtpw.SendKeys(password);

						IWebElement btnLogin = driver.FindElement(By.Id("passwordNext"));
						btnLogin.Click();

						Thread.Sleep(3000);

						IWebElement btncheck = driver.FindElement(By.XPath("//a[.=\"고급\"]"));
						if (btncheck != null)
						{
							btncheck.Click();
						}

						IWebElement btncheck2 = driver.FindElement(By.XPath("//a[contains(.,'로 이동(안전하지 않음)')]"));
						if (btncheck2 != null)
						{
							btncheck2.Click();
						}

						Thread.Sleep(5000);

						IWebElement btnPer = driver.FindElement(By.XPath("//*[@id=\"oauthScopeDialog\"]/div[3]/div[1]/content/span"));
						if (btnPer != null)
						{
							btnPer.Click();
						}

						Thread.Sleep(5000);

						IWebElement btnPer2 = driver.FindElement(By.XPath("//*[@id=\"submit_approve_access\"]/content/span"));
						if (btnPer2 != null)
						{
							btnPer2.Click();
						}

						Thread.Sleep(7000);

						IWebElement getPhotos = driver.FindElement(By.Id("linkPhoto"));
						getPhotos.Click();

						Thread.Sleep(7000);

						ICollection<IWebElement> imgPhotos = driver.FindElements(By.ClassName("googlePhotos"));
						Console.WriteLine(imgPhotos.Count);

						string strSrc = string.Empty;
						foreach (IWebElement item in imgPhotos)
						{
							Console.WriteLine(item.GetAttribute("src"));
							strSrc = strSrc + "\n" + item.GetAttribute("src");
						}

						using (System.IO.StreamWriter file = new System.IO.StreamWriter(@"C:\Users\김윤아\Desktop\test.txt"))
						{
							file.WriteLine(strSrc);
						}

						Thread.Sleep(7000);

					}
					catch (Exception ee)
					{
						Console.WriteLine(ee.Message);
						Console.ReadKey();

						throw;
					}

					driver.Quit();
				}
			}
			catch (Exception e)
			{
				Console.WriteLine(e.Message);
				Console.ReadKey();
				throw;
			}
		}
	}
}

```
Program.cs 파일

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="REDIRECTURL" value="http://localhost/study/googlephotos.asp"/>
    <add key="SCOPE" value="https://www.googleapis.com/auth/photoslibrary https://www.googleapis.com/auth/photoslibrary.readonly"/>
    <add key="ACCESSTYPE" value="offline"/>
    <add key="INCLUDEGRANTEDSCOPES" value="true"/>
    <add key="STATE" value="state_parameter_passthrough_value"/>
    <add key="RESPONSETYPE" value="code"/>
    <add key="CLIENTID" value="클라이언트id"/>
    <add key="LOGINID" value="구글계정"/>
    <add key="PWD" value="비번"/>
  </appSettings>
</configuration>
```
App.config 파일
