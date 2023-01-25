# Overview
Currently, tooltips are very simple and only in plain text assigned to each field. Sometimes a field might require more detailed information about the field, but the current tooltips are quite limited.

Using JavaScript tooltips allows the use of:
- A custom icon to indicate that a tooltip is present for this action.
- HTML in the content of the tooltip to allow more flexibility.

# Implementation

1. In the Form Designer, click the gear icon to open the **Form configuration** panel.
2. On the **General** tab, check **Enable AJAX mode**.
3. On the **Web References** tab, check **Include jQuery API and jQuery UI** libraries.
4. Copy anjd paste the following line of code in the text area:

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" />

<style>
    i{
        margin-left:5px;
        font-size: 16px;
    }
</style>
```

5. Close the **Form configuration** panel, then click the **JS** button in the Form Designer toolbar.
6. Copy and paste the following function in the text area:

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

This function takes 2 parameters: the label field ID and the tooltip message you want to add to the field.

7. In the JS `pageLoad` function, call the `setTooltip` for each additional tooltip you want to add to your label.

    **Example:**

```js
function pageLoad () {
    setTooltip("#REQUEST_FIRSTNAME_LABEL", "This is the field to set your first name");
    setTooltip("#REQUEST_LASTNAME_LABEL", "This is the field to set your last name ");
    setTooltip("#REQUEST_DATE_LABEL", "This field represents today's date. You can confirm today's date by going to this website: <br/><a target=\"_blank\" href=\"https://www.calendardate.com/todays.htm\">https://www.calendardate.com/todays.htm</a> ");
    setTooltip("#REQUEST_SUBJECT_LABEL", "Please input the subject of your request. The options are: <br /> - OPEN <br /> - ONGOING <br /> - CLOSE");
    setTooltip("#REQUEST_DESCRIPTION_LABEL", "Please insert a short description of your request");
}
```
