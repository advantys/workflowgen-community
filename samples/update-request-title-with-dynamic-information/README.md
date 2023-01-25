# Overview
This article shows you how to add dynamic information to the tile of a request.

For example have the request title have the first and last name of the requester: 
- `REQUEST# 134 - John Doe`
- `REQUEST# 135 - Jane Smith`

# Implementation

In the form designer code-behind, copy-paste the code below:

```cs
protected void Page_Load(object sender, EventArgs e)
{
    if(!IsPostBack)
        if(CurrentWorkflowActionName == "INITIATES")
            CURRENT_REQUEST.Text += " - " + REQUEST_FIRSTNAME.Text + " " + REQUEST_LASTNAME.Text;
    base.Page_Load(sender, e);
}
```
   > * Make sure the parameter `IsPostBack` is set before updating the value of CURRENT_REQUEST.Text, otherwise the title will keep adding the information at each page refresh.
   >
   > * The parameter `CurrentWorkflowActionName == ""` is not required but lets you control on which action the title will be updated.

