### Работа с GridView

## Болванка
```asp
<ext:GridPanel runat="server" ID="GrAllowedPerson">
	<Store>
		<ext:Store runat="server" ID="GrAllowedPersonStore">
			<Model>
				<ext:Model runat="server">
					<Fields>
						<ext:ModelField Name="PERSONNUMBER"></ext:ModelField>
						<ext:ModelField Name="PERSONFIO"></ext:ModelField>
						<ext:ModelField Name="PERSONPOST"></ext:ModelField>
					</Fields>
				</ext:Model>
			</Model>
		</ext:Store>
	</Store>
	<ColumnModel ID="ColumnModel6">
		<Columns>
			<ext:Column Text="ФИО" DataIndex="PERSONFIO"  runat="server" />
			<ext:Column Text="Должность" DataIndex="PERSONPOST" runat="server" />
		</Columns>
	</ColumnModel>
</ext:GridPanel>
```


```asp
// new entity block
var dbCntx = new Entities();
var admission = dbCntx.ADMISSION.Find(int.Parse(id));
var members = admission.MEMBER.Where(x => x.MEMBER_TYPEID == 2).Select(x => new {PERSONNUMBER = x.PERSONNUMBER ,PERSONFIO = x.PERSONFIO, PERSONPOST = x.PERSONPOST}).ToList();

GrAllowedPersonStore.Data = members;
GrAllowedPersonStore.DataBind();
```

## Команды для удаления и редактирования

```asp
<ext:CommandColumn runat="server" Width="60">
    <Commands>
        <ext:GridCommand Icon="Delete" CommandName="Delete">
            <ToolTip Text="Delete" />
        </ext:GridCommand>
        <ext:CommandSeparator />
        <ext:GridCommand Icon="NoteEdit" CommandName="Edit">
            <ToolTip Text="Edit" />
        </ext:GridCommand>
    </Commands>
    <Listeners>
        <Command Handler="Ext.Msg.alert(command, record.data.company);" />
    </Listeners>
</ext:CommandColumn>
```

Для добавления логики показа и работ кнопок (http://examples2.ext.net/#/GridPanel/Commands/Prepare_Toolbar/):

```asp
<ext:CommandColumn runat="server" Width="120">
    <Commands>
        <ext:GridCommand Icon="Delete" CommandName="Delete" Text="Delete" />
    </Commands>
    <PrepareToolbar Fn="prepare" />
    <Listeners>
		<Command Fn="AgreementAdmission" />
	</Listeners>
</ext:CommandColumn>
```

```javascript
var prepare = function (grid, toolbar, rowIndex, record) {
    var firstButton = toolbar.items.get(0);

    if (record.data.price < 50) {
        firstButton.setDisabled(true);
        firstButton.setTooltip("Disabled");
    }

    //you can return false to cancel toolbar for this record
};
var AgreementAdmission = function (record, commandColumn, recordIndex, a) {
	// todo сделать tooltip на кнопке, где указать кем вы являетесь 
    DB_Operation.AgreementAdmission(recordIndex.data.ID);
    
    recordIndex.data.ISREAD = "1";
    App.GPAdmission.getView().refreshNode(App.GPAdmission.getStore().indexOfId(recordIndex.data.ID));
}
```
```c#
[DirectMethod(Namespace = "DB_Operation")]
public void AgreementAdmission(string id)
{
	using (var dbCntx = new Entities())
    {
    	// your code here
	}
}
```

Для вывода Да/Нет в колонке
```asp
<ext:Column ID="Column48" runat="server" Text="Ознакомлен" DataIndex="IS_READ" Flex="1" Hidden="false" Width="10">
	<Renderer Fn="getIsReadValue" />
</ext:Column>
```
```javascript
var getIsReadValue = function (value, metadata, record) {
	return value ? "Да" : "Нет";
}
```
