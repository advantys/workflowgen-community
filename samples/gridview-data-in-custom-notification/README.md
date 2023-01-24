# Overview
This article shows you how to export a gridview content to display it inside an HTML table for your custom notification.

In the `GRIDVIEW_IN_CUSTOM_EMAIL` sample process, we are exporting the default gridview sample.

**Note:** The sample process and code example are provided as-is, you can adapt the code if you want a different style than a simple HTML table.

# Implementation

1. In the form designer, add a new textbox field. 
    1. This textbox will contain the HTML table of your gridview content.
    2. This textbox field can be hidden.
    3. Make sure the textbox has a Value Out data specified in the mapping.

2. In the form designer code-behind, add the following line of code:

Firstly, add a new event on the submit button, this event will be the one generating the HTML template of your gridview.
````
protected void Page_Load(object sender, EventArgs e) 
{ 
    base.Page_Load(sender, e); 
    submitButton.Click += new EventHandler(MySubmitButton_Click); 
} 
````

Secondly, add the method that will be called by the event on the submit button.

1. Create a variable that will contain the value of your HTML table and initiate it with the table header.
2. Use a loop to go across all the rows of your table, in this example the table id is `REQUEST_GRIDVIEW`.
3. During each loop you want to add a new row that will contain the value of each of your columns.
4. After looping all the rows, save the data in the new textbox field you added earlier.

````
protected void MySubmitButton_Click(object sender, EventArgs e) 
{ 
    
    var gvContent = "<table><tr><th>Title</th><th>Description</th><th>Price</th></tr>";
        
    foreach(System.Data.DataRow row in FormData.Tables["REQUEST_GRIDVIEW"].Rows)
    {	
        gvContent += "<tr><td>" + row["REQUEST_GRIDVIEW_TITLE"].ToString() + "</td><td>" + row["REQUEST_GRIDVIEW_DESCRIPTION"].ToString() + "</td><td>" + row["REQUEST_GRIDVIEW_PRICE"].ToString() + "</td></tr>";	
    }
    
    gvContent += "</table>";
    
    REQUEST_GRIDVIEW_CONTENT.Text = gvContent;
    
    this.SaveFormData(this.FormData, true); 
    SubmitToWorkflow(); 
}
````

3. In the data tab add your custom email template and use the value of your textbox field containing the HTML table.

    1. Make sure to give a style for your HTML table inside your email template.
    2. Use the macro <WF_PROCESS_INST_RELDATA_VALUE.(YOUR_DATA_NAME)> to retrieve the value of your data to be used in the email.

```
<!DOCTYPE html>
<html>
  <style>
    table, th, td {
      border:1px solid black;
    }
  </style>
  <body>

    <h2>Here is the process gridview content in the HTML table</h2>

    <WF_PROCESS_INST_RELDATA_VALUE.REQUEST_GRIDVIEW_CONTENT>

  </body>
</html>
```
 
 4. In the notification tab of your action, make an additional notification that will be using your custom email template.