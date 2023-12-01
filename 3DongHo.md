# Сформируйте контрол, визуализирующий часы

### MainWindow.xaml

```cs

<Window.DataContext>
    <local:WatchViewModel />
</Window.DataContext>
<Grid>
    <TextBlock Text="{Binding CurrentTime, StringFormat='HH:mm:ss'}"
               FontSize="36"
               HorizontalAlignment="Center"
               VerticalAlignment="Center" />
</Grid>
```

### MainWindow.xaml.cs

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
}
```

### WatchModel.cs

```cs
 public class WatchModel
 {
     public DateTime CurrentTime => DateTime.Now;
 }
```

### WatchViewModel.cs

```cs
public class WatchViewModel : INotifyPropertyChanged
{
    private readonly WatchModel _clockModel;
    private readonly DispatcherTimer _timer;

    public WatchViewModel()
    {
        _clockModel = new WatchModel();
        _timer = new DispatcherTimer { Interval = TimeSpan.FromSeconds(1) };
        _timer.Tick += Timer_Tick;
        _timer.Start();
    }

    public DateTime CurrentTime => _clockModel.CurrentTime;

    public event PropertyChangedEventHandler? PropertyChanged;

    private void Timer_Tick(object sender, EventArgs e)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(CurrentTime)));
    }
}
```
