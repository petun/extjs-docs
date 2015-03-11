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

Свой шаблон на выводе:

```asp
<ListConfig>
	<ItemTpl runat="server">
		<Html>
			<div class="list-item">
				<h3>{state}</h3>
				{nick:ellipsis(15)}, {price:usMoney}
			</div>
		</Html>    
	</ItemTpl>
</ListConfig>
```

Добавление элементов из кода - http://examples2.ext.net/#/Form/ComboBox/Items_Actions/
```asp
//Please note that inner items will be above store's items
this.ComboBox1.Items.Insert(0, new Ext.Net.ListItem("None", "-"));
this.ComboBox1.SelectedItems.Add(new Ext.Net.ListItem("-"));
```

Добавление элементов из клиента
```asp
<ext:Button runat="server" Text="Insert: client side">
	<Listeners>
		<Click Handler="#{ComboBox1}.insertRecord(1, { 
			Text  : 'Text1', 
			Value : 1 
			});
			#{ComboBox1}.setValue(1);
			this.disable();" />
		</Listeners>
	</ext:Button>
```

Очистка значения
```asp
App.TFDEPARTMENT.clearValue();
```



Другие функции
- addRecord: function (values)
- addItem: function (text, value)
- insertRecord: function (rowIndex, values)
- insertItem: function (rowIndex, text, value)
- removeByField: function (field, value)
- removeByIndex: function (index)
- removeByValue: function (value)
- removeByText: function (text)



### MultiCombo
```asp
<ext:MultiCombo runat="server" Width="260">
	<Items>
		<ext:ListItem Text="Item 1" Value="1" />
		<ext:ListItem Text="Item 2" Value="2" />
		<ext:ListItem Text="Item 3" Value="3" />
		<ext:ListItem Text="Item 4" Value="4" />
		<ext:ListItem Text="Item 5" Value="5" />
	</Items>
	
	<SelectedItems>
		<ext:ListItem Value="2" />
		<ext:ListItem Index="4" />
	</SelectedItems>
</ext:MultiCombo>

```
