### Direct Events

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
```

```asp
protected void Button_Click(object sender, DirectEventArgs e) {
  var e =  e.ExtraParams["Item"];
}
```