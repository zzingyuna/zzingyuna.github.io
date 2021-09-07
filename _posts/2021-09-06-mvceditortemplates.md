---
layout: post
---

# .NET MVC Editor Templates


Currency
```
@model decimal?

@(Html.Kendo().CurrencyTextBoxFor(m => m)      
      .HtmlAttributes(new {style="width:100%"})
      .Min(0)
)
```


Date
```
@model DateTime?

@(Html.Kendo().DatePickerFor(m => m))
```


DateTime
```
@model DateTime?

@(Html.Kendo().DateTimePickerFor(m => m))
```


Email
```
@model object 

@Html.TextBoxFor(model => model, new {@class="k-textbox" })
```


EmailAddress
```
@model object 

@Html.TextBoxFor(model => model, new {@class="k-textbox", type="email" })
```


GridForeignKey
```
@model object
           
@(
 Html.Kendo().DropDownListFor(m => m)        
        .BindTo((SelectList)ViewData[ViewData.TemplateInfo.GetFullHtmlFieldName("") + "_Data"])
)
```


Integer
```
@model int?

@(Html.Kendo().IntegerTextBoxFor(m => m)
      .HtmlAttributes(new { style = "width:100%" })
      .Min(int.MinValue)
      .Max(int.MaxValue)
)
```


Number
```
@model double?

@(Html.Kendo().NumericTextBoxFor(m => m)
      .HtmlAttributes(new { style = "width:100%" })
)
```


Password
```
@model object 

@Html.TextBoxFor(model => model, new {@class="k-textbox", type="password" })
```


String
```
@model object 

@Html.Kendo().TextBoxFor(model => model)
```


Time
```
@model DateTime?
           
@(Html.Kendo().TimePickerFor(m => m))
```


Url
```
@model object 

@Html.TextBoxFor(model => model, new {@class="k-textbox", type="url" })
```