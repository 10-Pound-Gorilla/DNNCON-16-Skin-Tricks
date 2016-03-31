
# Configure Viewport #

Using the meta tag we can set your page to to be displayed properly on mobile devices instead of just be a zoomed out version of your desktop design. By utilizing DNN's "Meta" skin object we can do this right in the skin.

```ASP
<dnn:META runat="server" Name="viewport" Content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" />
```





# Include Files #

Additional files can be included in your ascx skin file. This is particularly usefull if you have multiple skin layouts but they all have some elements that are the same e.g. Headers and Footers. This allows you to have to one place for the code that is shared accross all the different layouts.

```ASP
<!--#include file="inc/header.inc" -->

```

> One frequent way I use this is to keep all of the register tags in a file seperate from the skin layout and then included it.






# Client Resource Management #

This is a skin object that allows you to include Javascript and CSS files in the head of the document.

The path can be set to "SkinPath" (the path to the current skin) and "SharedScripts" (~/Resources/Shared/Scripts/) otherwise it can be removed and the full path can be set in the "FilePath" attribute.

You can also control the order these are loaded by utilizing the "Priority" attribute.

```ASP
<%@ Register TagPrefix="dnn" Namespace="DotNetNuke.Web.Client.ClientResourceManagement" Assembly="DotNetNuke.Web.Client" %>
<dnn:DnnJsInclude runat="server" FilePath="" PathNameAlias="" Priority="" />
<dnn:DnnCssInclude runat="server" FilePath="" Priority="" />
```

**JavaScript priorities:**
- Default: 100
- jQuery: 5
- jQuery UI: 10
- DnnXxml: 15
- DnnXmlJsParser: 20
- DnnXmlHttp: 25
- DnnXmlHttpJsXmlHttpRequest: 30
- DnnDomPositioning: 35
- DnnControls: 40
- DnnControlsLabelEdit: 45

**CSS priorities:**
- DefaultCss: 5
- ModuleCss: 10
- SkinCss: 15
- SpecificSkinCss: 20
- ContainerCss: 25
- SpecificContainerCss: 30
- PortalCss: 35

---
 > A great way to utilize this is with Google fonts. If you ever use them then you know you have to either use the link tag or import in your css. Well the problem with import is that it can prevent the rest of the site from loading untill it's done and the problem with the link tag is that it is supposed to be placed in the head. This allows you to do just that.

```ASP
<dnn:DnnCssInclude runat="server" FilePath="https://fonts.googleapis.com/css?family=Open+Sans:400,300,300italic,400italic,600,600italic,700,700italic,800,800italic|Libre+Baskerville:400,400italic,700" />
```






# Multiple Menus #

By utilizing the availaible attibutes "ExcludeNodes" & "IncludeNodes" you can create several different menus.

The most common use of this is with the main navigation usually at the top of the page and then a secondary menu of child pages on the side.

```ASP
<div id="primary-navigation"><dnn:Menu MenuStyle="Ten-Pound-DDR-Menu" runat="server" NodeSelector="*,0,5" ExcludeNodes="123,124,125,126,127"></dnn:MENU></div>
<div id="secondary-navigation"><dnn:Menu MenuStyle="Ten-Pound-DDR-Menu" runat="server" NodeSelector="*,0,5" IncludeNodes="123,124,125,126,127"></dnn:MENU></div>
```






# Desktop & Mobile Menus #

```ASP
<div id="navigation" class="hidden-xs"><dnn:Menu MenuStyle="Ten-Pound-DDR-Menu" runat="server" NodeSelector="*,0,5"></dnn:MENU></div>
<div id="mobile-navigation" class="visible-xs"><dnn:Menu MenuStyle="Ten-Pound-DDR-Menu-Mobile" runat="server" NodeSelector="*,0,5"></dnn:MENU></div>
```






# Conditional Content #

```ASP
<% if ( UserController.GetCurrentUserInfo().IsInRole("Administrators") ) { %>
	// Do Something
<% } else { %>
	// Do Something Else
<% } %>
```






# Dynamic Attributes #
```ASP
<%@ Control language="c#" AutoEventWireup="false" Explicit="True" Inherits="DotNetNuke.UI.Skins.Skin" %>
<%
	string pageName = PortalSettings.ActiveTab.TabName;
	int pageId = PortalSettings.ActiveTab.TabID;
	string logged = (Request.IsAuthenticated) ? "logged-in" : "logged-out";
	string admin = (UserController.GetCurrentUserInfo().IsInRole("Administrators")) ? "admin" : "";
	string bc = "bc-" + PortalSettings.ActiveTab.BreadCrumbs.Cast<DotNetNuke.Entities.Tabs.TabInfo>().First().TabID;
%>
```ASP
<div class="<%= logged %> <%= admin %> <%= bc %>" id="page-<%= pageId %>" data-name="<%= pageName %>">...</div>
```






# DNN Properties in JS Object #
[Full Object Code Available Here](https://github.com/10-Pound-Gorilla/DotNetNuke-Site-Properties-JS-Object)






# Resources #
https://github.com/10-Pound-Gorilla/DNNCON-15-Skin-Tricks
http://www.10poundgorilla.com/DNN/Skinning-Tool
http://boomerangdns.com/Computers/Specifications/DNNDotNetNukeStuff/DotNetNukeModuleTokens.aspx
