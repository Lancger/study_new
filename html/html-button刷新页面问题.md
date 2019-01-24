```
<button type="button">  这个原理是啥  为何加上了就不会强制刷新了


button的默认type 是submit,相当于是表单提交，然后会刷新页面
```

# 二、ajax  post
```
function batchTickets() {
    var cbx = $("input[name='select_id_cbx']:checked");
    var checkBoxValue = "";
    cbx.each(function () {
        checkBoxValue += $(this).val() + ",";
    });
    checkBoxValue = checkBoxValue.substring(0, checkBoxValue.length - 1);
    console.log(checkBoxValue);
    if (checkBoxValue == '') {
        alert("参数错误，请选择至少一项信息")
    }
    else
    {
        $.ajax({
            url: '/workflow/batch_tickets',
            type: 'POST',
            dataType: 'json',
            data:  {
                    "ticket_title": $("#ticket_title").val(),
                    "ticket_status": $("#ticket_status").val(),
                    "ticket_id_array": checkBoxValue,
                    "ticket_type_id": $("#ticket_type_id").val()
            },
            success: function (res) {
                $('#ticket_contlist_modal').hide();
                // $('#batch_contlist_modal').show();
                //使用modal('toggle')才可以正常显示拟态框
                $('#batch_contlist_modal').modal('toggle');
                $('#batch_contlist_modal').find('.modal-inner-content').html(res.msg);
            }
        })
    }
}

```
