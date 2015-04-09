### Forms How To
## Простой способ
```asp
<ext:TextField FieldLabel="First Name" AllowBlank="false" />
```

## Свои проверки с помощью VType
http://examples.ext.net/#/Form/Validation/Custom_VType/
http://docs.sencha.com/ext-js/4-1/#!/api/Ext.form.field.VTypes
```asp
<ext:TextField FieldLabel="Enter a value" Vtype="alphanum" />
<ext:TextField FieldLabel="Email" Vtype="email" />

// очень важно!!! для пустых значений
Vtype="listWorkCheck" ValidateBlank="True"
issuedUniq
```


```javascript
// свой метод обработки ....
Ext.apply(Ext.form.VTypes, {
    password : function (val, field) {
        if (field.initialPassField) {
            var pwd = Ext.getCmp(field.initialPassField);
            return pwd ? (val === pwd.getValue()) : false;
        }
        return true;
    },
    passwordText : "Passwords do not match"
});
```


```asp
<ext:Button runat="server" Text="Save">
    <DirectEvents>
        <Click OnEvent="SaveData"
               Before="return #{FormPanel1}.isValid();">
            <ExtraParams>
                <ext:Parameter
                        Name="values"
                        Value="#{FormPanel1}.getForm().getValues()"
                        Mode="Raw"
                        Encode="true"
                        />
            </ExtraParams>
        </Click>
    </DirectEvents>
</ext:Button>


protected void SaveData(object sender, DirectEventArgs e)
{
    var values = JSON.Deserialize<Dictionary<string, string>>(e.ExtraParams["values"]);
}
```

