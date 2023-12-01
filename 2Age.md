# Сформируйте контрол с вводом даты, определяющий совершеннолетие пользователя

### MainWindow.xaml

```xaml
<Window.DataContext>
    <local:UserViewModel/>
</Window.DataContext>
<Grid>
    <Grid.DataContext>
        <local:UserViewModel x:Name="ViewModel" />
    </Grid.DataContext>

    <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
        <TextBlock Text="Birthday:" />
        <DatePicker SelectedDate="{Binding User.DateOfBirth, Mode=TwoWay}" />
        <TextBlock Text="Age: " />
        <TextBlock Text="{Binding User.Age, Mode=OneWay}" />
    </StackPanel>
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

### UserModel.cs

```cs
public class UserModel : INotifyPropertyChanged
{
    private DateTime _dateOfBirth;
    private int _age;

    public DateTime DateOfBirth
    {
        get { return _dateOfBirth; }
        set
        {
            if (_dateOfBirth != value)
            {
                _dateOfBirth = value;
                CalculateAge();
                OnPropertyChanged(nameof(DateOfBirth));
                OnPropertyChanged(nameof(Age));
            }
        }
    }

    public int Age
    {
        get { return _age; }
        private set
        {
            if (_age != value)
            {
                _age = value;
                OnPropertyChanged(nameof(Age));
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void CalculateAge()
    {
        var today = DateTime.Today;
        var age = today.Year - DateOfBirth.Year;
        if (today.Month < DateOfBirth.Month || (today.Month == DateOfBirth.Month && today.Day < DateOfBirth.Day))
            age--;
        Age = age;
    }

    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### UserViewModel.cs

```cs
public class UserViewModel : INotifyPropertyChanged
{
    private UserModel _user;

    public event PropertyChangedEventHandler PropertyChanged;

    public UserModel User
    {
        get { return _user; }
        set
        {
            if (_user != value)
            {
                if (_user != null)
                    _user.PropertyChanged -= OnUserPropertyChanged;

                _user = value;

                if (_user != null)
                    _user.PropertyChanged += OnUserPropertyChanged;

                OnPropertyChanged(nameof(User));
                OnPropertyChanged(nameof(User.Age));
            }
        }
    }

    public UserViewModel()
    {
        User = new UserModel();
    }

    private void OnUserPropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        if (e.PropertyName == nameof(UserModel.DateOfBirth))
        {
            OnPropertyChanged(nameof(User.Age));
        }
    }

    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

```
