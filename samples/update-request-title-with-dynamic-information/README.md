# Overview
This article will show you how to add dynamic information to the title of a request.

For example, you can have the request title include the first and last names of the requester: 
- `REQUEST# 134 - John Doe`
- `REQUEST# 135 - Jane Smith`

# Implementation

In the Form Designer code-behind, copy and paste the code below:

```cs
protected void Page_Load(object sender, EventArgs e)
{
    if(!IsPostBack)
        if(CurrentWorkflowActionName == "INITIATES")
            CURRENT_REQUEST.Text += " - " + REQUEST_FIRSTNAME.Text + " " + REQUEST_LASTNAME.Text;
    base.Page_Load(sender, e);
}
```
   > * Make sure the `IsPostBack` parameter is set before updating the value of /CURRENT_REQUEST.Text/, otherwise the title will keep adding the information upon each page refresh.
   >
   > * The `CurrentWorkflowActionName == ""` parameter is not required, but lets you control on which action the title will be updated.
