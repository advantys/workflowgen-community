# Overview
Currently, the tooltips are really simple and only in plain text assigned to each field, some times a field would require more detailed information on the field, but the current tooltips are quite limited.

The JS tooltips allow the use of:
- An icon to represent a tooltip is present for this action.
- HTML in the content of the tooltip to allow more flexibility.

# Implementation

1. In the form designer click on the gear icon to open the Form configuration
2. In the General tab enable AJAX mode
3. In the Web References tab include jQuery API and jQuery UI libraries
4. Copy-paste the following line of code

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />

<style>
    i{
        margin-left:5px;
        font-size: 16px;
    }
</style>
```

5. Close the form configuration and click on the JS tab.
6. Copy-paste the following function

```js
function setTooltip(labelId, tooltipMessage)
{
    $(labelId).attr('title', '');
    
    $(labelId).tooltip({
        content: tooltipMessage,
        open: function (event, ui) {
            ui.tooltip.css("max-width", "500px");
        },
        close: function (event, ui) {
        ui.tooltip.hover(
    
            function () {
            $(this).stop(true).fadeTo(400, 1);
            },
    
            function () {
            $(this).fadeOut("400", function () {
                $(this).remove();
            });
            });
        }
    });
    $(labelId).append('<i class="fa fa-info-circle"></i>');
}
```

This function takes 2 parameters, the label field id and the tooltip message you want to add to the field.

7. In the JS pageLoad function you will call the setTooltip for each additional tooltip you want to add to your label.
8. Example:

```js
function pageLoad () {
    setTooltip("#REQUEST_FIRSTNAME_LABEL", "This is the field to set your first name");
    setTooltip("#REQUEST_LASTNAME_LABEL", "This is the field to set your last name ");
    setTooltip("#REQUEST_DATE_LABEL", "This field represent today dates, you may confirm today dates by going to this website: <br/><a target=\"_blank\" href=\"https://www.calendardate.com/todays.htm\">https://www.calendardate.com/todays.htm</a> ");
    setTooltip("#REQUEST_SUBJECT_LABEL", "Please input the subject of your request, the option are: <br /> - OPEN <br /> - ONGOING <br /> - CLOSE");
    setTooltip("#REQUEST_DESCRIPTION_LABEL", "Please insert a short description of your request");
}
```
