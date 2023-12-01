# Сформируйте контрол, находящий сумму 2х чисел

### SumModel

```cs
public class SumModel : INotifyPropertyChanged
{
    private int _number1;
    public int Number1
    {
        get => _number1;
        set
        {
            if (_number1 != value)
            {
                _number1 = value;
                OnPropertyChanged(nameof(Number1));
                CalculateSum();
            }
        }
    }

    private int _number2;
    public int Number2
    {
        get => _number2;
        set
        {
            if (_number2 != value)
            {
                _number2 = value;
                OnPropertyChanged(nameof(Number2));
                CalculateSum();
            }
        }
    }

    private int _sum;
    public int Sum
    {
        get => _sum;
        set
        {
            if (_sum != value)
            {
                _sum = value;
                OnPropertyChanged(nameof(Sum));
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    private void CalculateSum()
    {
        Sum = Number1 + Number2;
    }
}
```

### SumViewModel

```cs
class SumViewModel : INotifyPropertyChanged
{
    private SumModel _model;
    public SumModel Model
    {
        get => _model;
        set
        {
            if (_model != value)
            {
                _model = value;
                OnPropertyChanged();
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public SumViewModel()
    {
        Model = new SumModel();
    }
}
```

### MainView.xaml

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>

    <Label Content="Number 1:"/>
    <TextBox Grid.Row="0" Margin="5" Width="100" Text="{Binding Model.Number1, UpdateSourceTrigger=PropertyChanged}"/>

    <Label Grid.Row="1" Content="Number 2:"/>
    <TextBox Grid.Row="1" Margin="5" Width="100" Text="{Binding Model.Number2, UpdateSourceTrigger=PropertyChanged}"/>


    <Label Grid.Row="3" Content="Sum:"/>
    <TextBox Grid.Row="3" Margin="5" Width="100" Text="{Binding Model.Sum}" IsReadOnly="True"/>
</Grid>

```

### MainView.xaml.cs

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SumViewModel viewModel = new SumViewModel();
        DataContext = viewModel;
    }
}
```
