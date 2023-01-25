# Overview
When a form gets big, it can be quite long for a user to scroll down to their section which also makes the usage less user-friendly.

A solution is to implement a section collapse, to reduce the amount the user needs to scroll and to only expand the sections that are required for that specific user.

# Implementation

1. In the form designer, add a new section.
    1. In our process sample we added the section `FORM`.

2. Add one textbox field to this new section, this field will contain which section should be collapsed.
    1. In our process sample, the textbox is named `FIELDS_COLLAPSED`.

3. Click on the gear icon to open the Form configuration

4. In the General tab enable the AJAX mode.

5. Click on the Web References tab

6. Includes the jQuery API and jQuery UI libraries

7. Add the following stylesheet in the Web References

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />
```

8. Go onto the JS tab of the form designer
9. Copy-paste the following code:

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

In the function `pageLoad` we hide the section `FORM` with JavaScript

In the function `setCollapse` we manage the form collapse with the field `FORM_FIELDS_COLLAPSED`

If your section or field has a different ID, you will need to change the reference in the JS function.