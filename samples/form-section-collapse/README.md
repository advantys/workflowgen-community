# Overview
When a form gets big, it can be quite long for a user to scroll down to their section, which makes the usage less user-friendly.

One solution is to implement a section collapse, which will reduce how much the user needs to scroll and only expand the sections that are required for that specific user.

# Implementation

1. In the form designer, add a new section. In our process sample we added the `FORM` section.

2. Add one textbox field to this new section; this field will contain a list of sections to be collapsed. In our process sample, the textbox is named `FIELDS_COLLAPSED`.

3. Click the gear icon to open the **Form configuration** panel.

4. On the **General** tab, check **Enable AJAX mode**.

5. On the **Web References** tab, check **Include the jQuery API and jQuery UI libraries**.

7. Add the following stylesheet in the text area:
``` html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
```
8. In the Form Designer, click **JS** in the toolbar to open the JavaScript code editor.

9. Copy and paste the following code in the text area:

```js    
function pageLoad () {
    $('#FORM').hide();

    setCollapse();
}

function setCollapse() {
    $("div.SectionHeader").append('<i class="fas fa-minus" style="float: left; margin-right:10px; font-size: 21px; margin-top: 2px"></i>');

    // Collapse the sections defined in HIDDEN_FIELDS_COLLAPSED
    if ($("#FORM_FIELDS_COLLAPSED").val() !== "") {
        var sections = $("#FORM_FIELDS_COLLAPSED").val().split(",");
        for (i = 0; i < sections.length; i++) {
            $("#" + $.trim(sections[i]) + " > div.SectionHeader").find("i").attr("class", "fas fa-plus");
            $("#" + $.trim(sections[i])).find("div.Fields").css("display", "none");
        }
    }

    $("div.SectionHeader").click(function () {
        var $fields = $(this).parent().find("div.Fields");
        if ($fields.css("display") == "none")
        {
            $(this).find("i").attr("class","fas fa-minus")
        }
        else
        {
            $(this).find("i").attr("class","fas fa-plus")
        }
        $(this).parent().find("div.Fields").toggle("blind", 500);
    }).attr("title", "Click to collapse or expand").css("cursor", "pointer");
}
```

In the `pageLoad` section, the `FORM` section is hidden with JavaScript.

In the `setCollapse` fuction, the form collapse is managed by the `FORM_FIELDS_COLLAPSED` field.

If your section or field has a different ID, you'll need to change the reference in the JavaScript function.

9. For every web form action, add the `FORM_FIELDS_COLLAPSED` parameter with a comma-separated list of section IDs for the sections you want to display collapsed when the form is first loaded.
