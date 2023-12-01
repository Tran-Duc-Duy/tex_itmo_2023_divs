# Сформируйте контрол со списком дел, которые можно отметить, как завершенные

### MainWindow.xaml

```xaml
<Window.DataContext>
    <local:TodoListViewModel/>
</Window.DataContext>
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>

    <ListBox Grid.Row="0" ItemsSource="{Binding Todos}" Margin="5">
        <ListBox.ItemTemplate>
            <DataTemplate>
                <StackPanel Orientation="Horizontal">
                    <CheckBox IsChecked="{Binding IsCompleted}" Margin="0,0,5,0"/>
                    <TextBlock Text="{Binding TaskName}"/>
                </StackPanel>
            </DataTemplate>
        </ListBox.ItemTemplate>
    </ListBox>

    <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="5">
        <TextBox x:Name="TaskNameTextBox" Width="150" Margin="0,0,5,0"/>
        <Button Width="80" Content="Add Task" Click="AddTask_Click"/>
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

    private void AddTask_Click(object sender, RoutedEventArgs e)
    {
        var taskName = TaskNameTextBox.Text;

        // Tạo một mục công việc mới và thêm vào danh sách
        var newTask = new TodoModel
        {
            TaskName = taskName,
            IsCompleted = false
        };

        var viewModel = (TodoListViewModel)DataContext;
        viewModel.Todos.Add(newTask);

        // Xóa nội dung của ô nhập tên công việc
        TaskNameTextBox.Text = string.Empty;
    }
}
```

### TodoModel

```cs
public class TodoModel : INotifyPropertyChanged
    {
        private string _taskName;
        private bool _isCompleted;

        public string TaskName
        {
            get { return _taskName; }
            set
            {
                if (_taskName != value)
                {
                    _taskName = value;
                    OnPropertyChanged();
                }
            }
        }

        public bool IsCompleted
        {
            get { return _isCompleted; }
            set
            {
                if (_isCompleted != value)
                {
                    _isCompleted = value;
                    OnPropertyChanged();
                }
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged([System.Runtime.CompilerServices.CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
```

### TodoListViewModel

```cs
public class TodoListViewModel : INotifyPropertyChanged
{
    private ObservableCollection<TodoModel> _todos;

    public ObservableCollection<TodoModel> Todos
    {
        get { return _todos; }
        set
        {
            if (_todos != value)
            {
                _todos = value;
                OnPropertyChanged();
            }
        }
    }

    public TodoListViewModel()
    {
        Todos = new ObservableCollection<TodoModel>();
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```
