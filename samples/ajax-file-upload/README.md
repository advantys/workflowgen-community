This article shows you how to upload multiple attachments using the [AjaxFileUpload](https://ajaxcontroltoolkit.devexpress.com/AjaxFileUpload/AjaxFileUpload.aspx) in the AJAX Control Toolkit from DevExpress.
It also has a drag-and-drop functionality for uploading files.

**Note:** The sample process and code example are provided as-is and are implemented based on this component. You can adapt the code if you want to integrate with another control. You might need to get the proper license if you want to use the AJAX Control Toolkit in production. Please contact DevExpress for this.

## Prerequisites:

1. You must install the [AJAX Control Toolkit](https://www.devexpress.com/Products/AJAX-Control-Toolkit/) on the web application server.


2. Copy the `AjaxControlToolkit.dll` file located in `C:\Users\[yourUser]\Documents\ASP.NET AJAX Control Toolkit\Bin\` to the `\wfgen\wfapps\webforms\bin` folder

    **NOTE**: In Visual Studio, you can also use **NuGet Console Manager** to [install the kit](https://www.nuget.org/packages/AjaxControlToolkit/18.1.1) and get the `AjaxControlToolkit.dll` file.

3. Create a `web.config` file in the `\wfgen\wfapps\webforms\` folder (if it doesn't already exist) and add the following configurations:

```xml
    <?xml version="1.0"?>
    <configuration>
        <system.webServer>
            <handlers>
                <add name="AjaxFileUploadHandler" verb="*"
        path="AjaxFileUploadHandler.axd"
        type="AjaxControlToolkit.AjaxFileUploadHandler, 
          AjaxControlToolkit"/>
            </handlers>
        </system.webServer>
    </configuration>
```

## Installation
1. Download the [AjaxFileUpload.txt](AjaxFileUpload.txt) file and copy it to `\wfgen\App_Data\Templates\Forms\[language]\Custom\fields`.

2. In the form designer, create the file process data(s) with the names `FILE1, FILE2, ..., FILEN` (as many as you need). 

    **Note:** The code-behind is referencing to `FILE1`, `FILE2`, etc. This is why you have to name it this way.

3. In the Form Designer, add the **AjaxFileUpload** tool (located under **Custom** in the **Tools** drop-down list) to one of the form sections.<br />
![image](https://user-images.githubusercontent.com/42981614/121242509-a8dbd580-c86a-11eb-9625-277f35eca889.png)

4. In the form designer code-behind, copy-paste the code below:

```cs
protected void Page_Load(object sender, EventArgs e)
{
     base.Page_Load(sender, e);
	    
     if (!Page.IsPostBack)
     {
         // ** Save the storagepath in cookie for later access by AjaxFileUpload_UploadComplete()
         Response.Cookies.Add(new HttpCookie("storagepath", this.StoragePath));
     }

     // ** Add our custom file attachments management function on form submit
     submitButton.Click += new EventHandler(SubmitButton_Click_HandleFileUpload);
}
	
/// <summary>
/// UploadComplete event called by the AjaxFileUpload control for each file upload
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
protected void AjaxFileUpload_UploadComplete(object sender, AjaxControlToolkit.AjaxFileUploadEventArgs e)
{
     string storagePath = Request.Cookies["storagepath"].Value;
     string fileUploadsPath = storagePath + @"\fileUploads\";

     if (!System.IO.Directory.Exists(fileUploadsPath))
     {
         System.IO.Directory.CreateDirectory(fileUploadsPath);
     }

     string filePath = fileUploadsPath + e.FileName;

     AjaxControlToolkit.AjaxFileUpload fu = sender as AjaxControlToolkit.AjaxFileUpload;

     // ** Save the file in the action instance folder e.g. 
     // e.g. \wfgen\App_Data\Files\EFormAspx\2018\03\13\92\1\1da5ab5d-c8f0-496a-90e4-ab87f60fda74\fileUploads
     fu.SaveAs(filePath);
}

/// <summary>
/// File attachments management function on form submit
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
protected void SubmitButton_Click_HandleFileUpload(object sender, EventArgs e)
{
     string fileUploadsPath = this.StoragePath + @"\fileUploads\";

     if (System.IO.Directory.Exists(fileUploadsPath))
     {
         System.IO.DirectoryInfo di = new System.IO.DirectoryInfo(fileUploadsPath);

         int i = 0;

         // For each file attachments found in \fileUploads, update the FILE1..N in form data
         foreach (System.IO.FileInfo fi in di.GetFiles())
         {
             i++;

             // Create the FILE1..N column if missing in formdata
             if (!this.FormData.Tables[0].Columns.Contains("FILE" + i))
             {
                 this.FormData.Tables[0].Columns.Add(new System.Data.DataColumn("FILE" + i, Type.GetType("System.String")));
             }

             // Set the file attachment path (format: fileuploads\myfile.txt) to the corresponding FORM1..N in formdata
             this.FormData.Tables[0].Rows[0]["FILE" + i] = @"fileUploads\" + fi.Name;
         }

         // Save the updated formdata
         SaveFormData(this.FormData);
     }
}
```

5. In the desired action, add the OUT parameters mapping,
e.g. `FILE1 : OUT : FILE1, FILE2 : OUT : FILE2, …, FILEN : OUT : FILEN`<br />
![502d6b103be85640b2f4eccff0ee6e425837e8ef_2_609x500](https://user-images.githubusercontent.com/42981614/121242648-ce68df00-c86a-11eb-92d5-14e6ca76a07b.gif)

You should have something like this after adding all `FILEN` parameters:<br />
![image](https://user-images.githubusercontent.com/42981614/121242632-cad55800-c86a-11eb-9081-6e99ff0b389f.png)


[Sample Process](2_LEVELS_APP_FILE_UPLOAD_FDv1.xml)


Documentation on the AjaxFileUpload control (Properties, Methods, Events and more) can be found at [https://github.com/DevExpress/AjaxControlToolkit/wiki/AjaxFileUpload](https://github.com/DevExpress/AjaxControlToolkit/wiki/AjaxFileUpload).
