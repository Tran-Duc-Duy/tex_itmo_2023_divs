Tìm từ - Сформируйте контрол, находящий все слова, начинающиеся с выбранной части

### WordModel

```cs
namespace wordFinder
{
    public class WordModel
    {
        public string Word { get; set; }
    }
}
```

### WordFinderViewModel

```cs
using System.Collections.ObjectModel;
using System.ComponentModel;

namespace wordFinder
{
    public class WordFinderViewModel : INotifyPropertyChanged
    {
        private string _searchPrefix;
        private string _textInput;
        private ObservableCollection<WordModel> _foundWords;

        public string SearchPrefix
        {
            get { return _searchPrefix; }
            set
            {
                _searchPrefix = value;
                OnPropertyChanged(nameof(SearchPrefix));
                FindWords();
            }
        }

        public string TextInput
        {
            get { return _textInput; }
            set
            {
                _textInput = value;
                OnPropertyChanged(nameof(TextInput));
                FindWords();
            }
        }

        public ObservableCollection<WordModel> FoundWords
        {
            get { return _foundWords; }
            set
            {
                _foundWords = value;
                OnPropertyChanged(nameof(FoundWords));
            }
        }

        public WordFinderViewModel()
        {
            FoundWords = new ObservableCollection<WordModel>();
        }

        private void FindWords()
        {
            string[] words = GetWordsFromSource();
            FoundWords.Clear();

            foreach (string word in words)
            {
                if (SearchPrefix != null && word.StartsWith(SearchPrefix, StringComparison.OrdinalIgnoreCase))
                {
                    FoundWords.Add(new WordModel { Word = word });
                }
            }
        }

        private string[] GetWordsFromSource()
        {
            if (string.IsNullOrEmpty(TextInput))
            {
                return new string[0];
            }

            string[] words = TextInput.Split(' ', StringSplitOptions.RemoveEmptyEntries);
            return words;
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

### MainWindow.xaml

```XAML
<Window x:Class="wordFinder.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wordFinder"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <StackPanel>
        <TextBox Text="{Binding TextInput}" Width="200" Margin="0 0 0 10"/>
        <TextBox Text="{Binding SearchPrefix}" Width="200" Margin="0 0 0 10"/>
        <Button Content="Find Words" Command="{Binding FindWordsCommand}"/>
        <ListBox ItemsSource="{Binding FoundWords}"  DisplayMemberPath="Word" Width="200" Height="150"/>
    </StackPanel>
</Window>
```

### MainWindow.xaml.cs

```xaml
using System.Windows;

namespace wordFinder
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            WordFinderViewModel viewModel = new WordFinderViewModel();
            DataContext = viewModel;
        }
    }
}
```
