# extjs-docs

## Работа с данными

```с#
List<object> data = new List<object>();
this.CitiesStore.DataSource = data; // data - IEnumerable
this.CitiesStore.DataBind();
```


## Элементы
### Combobox
```asp
<ext:ComboBox 
        ID="ComboBox2" 
        runat="server" 
        TypeAhead="true" 
        QueryMode="Local"
        ForceSelection="true" 
        TriggerAction="All" 
        DisplayField="name" 
        ValueField="id"
        EmptyText="Loading..." 
        ValueNotFoundText="Loading...">
        <Store>
            <ext:Store 
                runat="server" 
                ID="CitiesStore" 
                AutoLoad="false" 
                OnReadData="CitiesRefresh">               
                <Model>
                    <ext:Model runat="server" IDProperty="Id">
                        <Fields>
                            <ext:ModelField Name="id" Type="String" ServerMapping="Id" />
                            <ext:ModelField Name="name" Type="String" ServerMapping="Name" />
                        </Fields>
                    </ext:Model>
                </Model>
                <Listeners>
                    <Load Handler="#{ComboBox2}.setValue(#{ComboBox2}.store.getAt(0).get('id'));" />
                </Listeners>
            </ext:Store>
        </Store>    
    </ext:ComboBox>
```

Можно просто элементы добавить:
```asp
<Items>
  <ext:ListItem Text="Belgium" Value="BE" />
  <ext:ListItem Text="Brazil" Value="BR" />
</Items>
```
