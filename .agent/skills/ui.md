<skill name="ui">
<rules>
- All ViewModels inherit from ModelBase (which extends Stylet.Screen) — never plain class
- Use IWindowManager.ShowWindow / ShowDialog for navigation — never instantiate views directly
- Bind commands via Stylet s:Action.Target — do not wire click handlers in code-behind
- Use HandyControl Growl for notifications (Growl.Error / Growl.Success) with Global.TOKEN_MAIN
- Theme changes go through Theme.Change(type); Language changes through Language.Change(type)
- XAML resource dictionaries live in Resource/ — do not hardcode colours or strings inline
- Language strings are accessed via Language.Get("strKey") — never hardcode English UI text
- PropertyChanged.Fody auto-generates INotifyPropertyChanged — no manual OnPropertyChanged needed
</rules>
<patterns>
// Open a new window from a ViewModel
Manager.ShowWindow(VMMain);

// Localised string
Language.Get("strmsgSearchStringIsEmpty")

// Growl notification (success)
Growl.Success(Language.Get("strmsgDone"), Global.TOKEN_MAIN);

// Growl notification (error)
Growl.Error(Language.Get("strmsgErr") + msg, Global.TOKEN_MAIN);

// Read current settings
Settings settings = Settings.Read();
</patterns>
<anti_patterns>
- Never use MessageBox.Show — use Growl or HandyControl Dialog
- Never put business/download logic in code-behind (.xaml.cs) — keep it in ViewModel
- Never hardcode path strings in XAML — bind to ViewModel properties
- Never manually raise PropertyChanged — let PropertyChanged.Fody handle it
- Never block async void ViewModel methods with .Result or .Wait()
</anti_patterns>
</skill>
