# Overview
This article shows you how to export the content of a gridview in order to display it inside an HTML table for your custom notification.

In the `GRIDVIEW_IN_CUSTOM_EMAIL` sample process, we'll be exporting the default gridview sample.

> **Note:** The sample process and code example are provided as-is. You can adapt the code if you want a different style instead of a simple HTML table.

# Implementation

1. In the form designer, add a new textbox field, which will contain the HTML table of your gridview content.
   > * This textbox field can be hidden.
   >
   > * Make sure the textbox has a `Value Out` data specified in the mapping.

2. In the Form Designer code-behind, add the following code to create new event on the **Submit** button. This will be the event that generates your gridview's HTML template.

```cs
protected void Page_Load(object sender, EventArgs e) 
{ 
    base.Page_Load(sender, e); 
    submitButton.Click += new EventHandler(MySubmitButton_Click); 
} 
```

2. Add the method that will be called by the event on the submit button. To do this:

    1. Create a variable that will contain the value of your HTML table and initiate it with the table header.
    2. Use a loop to go across all the rows of your table. In this example the table ID is `REQUEST_GRIDVIEW`.
    3. During each loop, you want to add a new row that will contain the value of each of your columns.
    4. After looping all the rows, save the data in the new textbox field you added earlier.

```cs
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
```

3. On the **Data** tab, add your custom email template and use the value of your textbox field that contains the HTML table.

    1. Make sure to give a style for your HTML table inside your email template.
    2. Use the `<WF_PROCESS_INST_RELDATA_VALUE.(YOUR_DATA_NAME)>` macro  to retrieve the value of your data to be used in the email.

```html
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
 
 4. On your action's **Notification** tab, create an additional notification that will use your custom email template.
