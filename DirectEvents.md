### Direct Events

```asp
<ext:Button runat="server" Text="Click Me">
	<DirectEvents>
		<Click OnEvent="Button_Click">
			<ExtraParams>
				<ext:Parameter Name="Item" Value="My param" />
			</ExtraParams>
			<EventMask ShowMask="true" />
		</Click>
	</DirectEvents>
</ext:Button>
```

```asp
protected void Button_Click(object sender, DirectEventArgs e) {
  var e =  e.ExtraParams["Item"];
}
```