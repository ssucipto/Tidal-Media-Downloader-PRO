<skill name="download">
<rules>
- All Tidal HTTP calls go through TidalLib.Client.* — never call Tidal endpoints directly
- All async ViewModel methods must be async void with try/catch or use Growl.Error for user feedback
- Download jobs run on ThreadTool thread pool — never block the UI thread
- File paths are built via Tools.GetAlbumPath / GetTrackPath / GetVideoPath — do not construct paths ad hoc
- TrackTask and VideoTask own their own lifecycle; ViewModel only creates and observes them
- ProgressHelper.SetStatus / SetProgress are thread-safe; call from worker thread is fine
- Always check Config.CheckExist before overwriting a file
- Metadata tagging uses TagLib — apply after full download, not during
</rules>
<patterns>
// Create a TrackTask (worker auto-starts in constructor)
Items.Add(new TrackTask(track, Items.Count + 1, Settings, RecieveDownloadOver, album));

// Notify parent on completion
public delegate void TELL_PARENT_OVER();
public TELL_PARENT_OVER TellParentOver;
// call: TellParentOver?.Invoke();

// Growl error from ViewModel
Growl.Error(Language.Get("strmsgSearchErr") + msg, Global.TOKEN_MAIN);
</patterns>
<anti_patterns>
- Never await inside ThreadTool.AddWork lambdas — use synchronous methods or Task.Run inside
- Never show MessageBox directly — use HandyControl Growl or Dialog
- Never store plaintext credentials in Settings.json — use UserSettings with EncryptHelper
- Never add NuGet packages that conflict with Costura.Fody embedding
</anti_patterns>
</skill>
