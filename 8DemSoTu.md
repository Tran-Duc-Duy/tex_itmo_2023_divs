# Сформируйте контрол, который считает количество слов во вставленном тексте

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
    private void CountButton_Click(object sender, RoutedEventArgs e)
    {
        string text = txtText.Text;
        if (!string.IsNullOrEmpty(text))
        {
            int wordCount = CountWords(text);
            txtResult.Text = $"Word count: {wordCount}";
        }
        else
        {
            txtResult.Text = "Please enter some text";
        }
    }

    private int CountWords(string text)
    {
        string[] words = text.Split(new char[] { ' ', '\t', '\n', '\r' }, StringSplitOptions.RemoveEmptyEntries);
        return words.Length;
    }
}
```

```xml

<Window x:Class="wordFinder.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wordFinder"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="Text:" />
            <TextBox x:Name="txtText" Width="200" Margin="0 5" />
            <Button Content="Count Words" Click="CountButton_Click" />
            <TextBlock x:Name="txtResult" Margin="0 10" />
        </StackPanel>
    </Grid>
</Window>

```
