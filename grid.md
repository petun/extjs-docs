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
//todo - забить все MEMBER_TYPEID в справочник или еще куда...

GrAllowedPersonStore.Data = members;
GrAllowedPersonStore.DataBind();
```

