# extjs-docs

## Работа с данными

```asp
List<object> data = new List<object>();
this.CitiesStore.DataSource = data; // data - IEnumerable
this.CitiesStore.DataBind();

// или вариант с БД на старом Oracle
OracleCommand command = connection.CreateCommand();
command.CommandText = sql;
command.Parameters.AddWithValue(":PERSONNUMBER", CBPersonSelect.Value);
command.Connection.Open();
OracleDataReader dr = command.ExecuteReader();
StorePerson.DataSource = dr;
StorePerson.DataBind();
```

Связь Model в ASPX
```asp
 <asp:SqlDataSource ID="DSPersonList" runat="server" ConnectionString="<%$ ConnectionStrings:OraConnStrISM %>"
                ProviderName="<%$ ConnectionStrings:OraConnStrISM.ProviderName %>" SelectCommand="SELECT 1,2 FROM DUAL"></asp:SqlDataSource>
```

```asp
<ext:Store 
	AutoLoad="True" 
	DataSourceID="DSPersonList"
```	


## Элементы
### Combobox
```asp
<ext:ComboBox 
        ID="ComboBox2" 
        FieldLabel="Выберите сотрудника"
        LabelAlign="Top"
        runat="server" 
        MultiSelect="true" // FIXIT IN CODE - надо или не надо выделение нескольких элементов
        TypeAhead="true" 
        Width="300" 
        QueryMode="Local"
        ForceSelection="true" 
        TriggerAction="All" 
        DisplayField="name" // очень важен регистр .. он должен совпадать с ModelField::Name
        ValueField="id" // очень важен регистр .. он должен совпадать с ModelField::Name
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

### Наши готовые решения
Выбор подразделения
```asp
<ext:ComboBox ID="TFDEPARTMENT" MarginSpec="0 0 20 0"
                                                FieldLabel="Подразделение: <span class='StarCls'>*</span>"
                                                LabelWidth="120"
                                                LabelSeparator=""
                runat="server" 
                DisplayField="label" 
                ValueField="value"
                TypeAhead="false"
                Width="455"
                PageSize="10"
                HideBaseTrigger="true"
                                  Note="<span style='color: rgb(195, 89, 89);'>Введите наименование подразделения, и выберите его из списка</span>"
                MinChars="3"
                TriggerAction="Query">
                <ListConfig  LoadingText="Поиск подразделения...">
                    <ItemTpl ID="ItemTpl1" runat="server">
                        <Html>
                            <div>
							    <h4 style="margin: 0;">{SIMPLENAME}</h4>
                                <p style="margin: 0; line-height: 100%;"><small>{INFO}</small></p>
						    </div>
                        </Html>
                    </ItemTpl>
                </ListConfig>
                <Triggers>
                   <ext:FieldTrigger Icon="Clear" Tag="clear" Qtip="Очистить поле" />
                   <ext:FieldTrigger Icon="Ellipsis" Tag="pickerdepartment" Qtip="Выбрать подразделение" />
                </Triggers>
                <Listeners>
                   <Blur Handler="SapHR.checkComboBoxValue(this)" />
                   <TriggerClick Handler="SapHR.triggerDepartmentComboClick(this, trigger, tag, false, 1);" />
                </Listeners>
                <Store>
                    <ext:Store ID="Store3" runat="server" AutoLoad="false">
                        <Proxy>
                            <ext:JsonPProxy Url="http://example:4444/Department/Search/">                                 
                                 <Reader>
                                <ext:JsonReader Root="departments" TotalProperty="totalCount" />
                            </Reader>                                
                            </ext:JsonPProxy>                            
                        </Proxy>
                        <Model>
                            <ext:Model ID="Model16" runat="server">
                                <Fields>
                                    <ext:ModelField Name="SIMPLENAME" />
                                    <ext:ModelField Name="MANAGERFIO" />
                                    <ext:ModelField Name="PARENTNAME" />
                                    <ext:ModelField Name="INFO" />                                    
                                    <ext:ModelField Name="value" />
                                    <ext:ModelField Name="label" />                                   
                                </Fields>
                            </ext:Model>                            
                        </Model>
                    </ext:Store>
                </Store>
            </ext:ComboBox>

             <%-- Окно выбора подразделения--%>
            <ext:Window ID="DepartmentSelectWindow" runat="server" Hidden="true" Title="" MaxWidth="1024" Cls="style_window"
                MaxHeight="600" Modal="true" Width="1024" Height="500" MinWidth="500" MinHeight="500"
                Closable="true" Maximizable="true" Maximized="false" Layout="FitLayout" Constrain="true">
                <Items>
                    <ext:TreePanel 
                        ID="TreePanel1" 
                        runat="server" 
                        Height="500"
                        Width="200" 
                        Margins="10"
                        Border="false"
                        RootVisible="false">
                        <Store>
                        <ext:TreeStore ID="TreeStore1" runat="server" AutoSync="True">
                            <Proxy>
                                <ext:JsonPProxy Url="http://example:4444/Department/Tree/?includeIds=50210774&includeIds=50192392&includeIds=50009788&includeIds=50009842&includeIds=50009868&includeIds=50048816&includeIds=50049126&includeIds=50052367&includeIds=50009193&includeIds=50009194&includeIds=50068971&includeIds=50001260&includeIds=50009441&includeIds=50009582&includeIds=50238822&includeIds=50009628&includeIds=50009124&includeIds=50009544&includeIds=50009662">
                                    <Reader>
                                        <ext:JsonReader Root="departments" TotalProperty="totalCount" IDProperty="value" />
                                    </Reader>                                
                                </ext:JsonPProxy>                            
                            </Proxy>
                            <Fields>
                                    <ext:ModelField Name="SIMPLENAME" />
                                    <ext:ModelField Name="PARENTNAME" />
                                    <ext:ModelField Name="INFO" />                                    
                                    <ext:ModelField Name="value" />
                                    <ext:ModelField Name="label" />   
                                </Fields>
                        </ext:TreeStore>
                        </Store>
                        <ColumnModel ID="ColumnModel5">
                            <Columns>
                                <ext:Column ID="Column43" DataIndex="value" runat="server" Text="ID" Hidden="true" />
                                <ext:TreeColumn ID="TreeColumn3" DataIndex="label" runat="server" Text="Подразделение" Flex="2">
                                </ext:TreeColumn>
                            </Columns>
                        </ColumnModel>
                        <Root>
                            <ext:Node NodeID="50000026" Text="ОМК">
                            </ext:Node>
                        </Root>

                        <SelectionModel>
                            <ext:TreeSelectionModel ID="TreeSelectionModel3" runat="server" Mode="Single">
                            </ext:TreeSelectionModel>
                        </SelectionModel>
                        <Listeners>
                            <ItemDblClick Handler="SapHR.selectDepartment(record.data)" />
                        </Listeners>
                    </ext:TreePanel>   
                </Items>
            </ext:Window>
```      

Выбор сотрудника
```asp
<ext:ComboBox ID="TrFOtvet"
                      FieldLabel="ФИО"
                      runat="server"
                      DisplayField="FIO"
                      ValueField="PERSONNUMBER"
                      TypeAhead="false"
                      PageSize="10"
                      HideBaseTrigger="true"
                      Note="Фамилия или табельный сотрудника"
                      MinChars="3"
                      TriggerAction="Query">
            <ListConfig LoadingText="Поиск сотрудников...">
                <ItemTpl ID="ItemTpl2" runat="server">
                    <Html>
                        <div class="search-item">
                            <h4 style="margin: 0;">{FIO}</h4>
                            <p style="margin: 0; line-height: 100%;"><small>{PERSONRANKNAME}, {DEPARTMENTNAME}</small></p>
                        </div>
                    </Html>
                </ItemTpl>
            </ListConfig>
            <Store>
                <ext:Store ID="Store4" runat="server" AutoLoad="false">
                    <Proxy>
                        <ext:JsonPProxy Url="http://example:4444/Person/Search/">
                            <Reader>
                                <ext:JsonReader Root="persons" TotalProperty="totalCount" />
                            </Reader>
                        </ext:JsonPProxy>
                    </Proxy>
                    <Model>
                        <ext:Model ID="Model17" runat="server">
                            <Fields>
                                <ext:ModelField Name="PERSONNUMBER" />
                                <ext:ModelField Name="FIO" />
                                <ext:ModelField Name="PHOTO" />
                                <ext:ModelField Name="PERSONRANKNAME" />
                                <ext:ModelField Name="PERSONEMAIL" />
                                <ext:ModelField Name="PERSONPHONE" />
                                <ext:ModelField Name="DEPARTMENTNAME" />
                            </Fields>
                        </ext:Model>
                    </Model>
                </ext:Store>
        </Store>
    </ext:ComboBox>
```

### GridView - основы работы

