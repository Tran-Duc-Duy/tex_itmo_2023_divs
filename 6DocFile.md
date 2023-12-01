# Сформируйте контрол, который выводит весь текст файла из переданного пути

### MainWindow.xaml.cs

```cs
using System.IO;
using System.Windows;

namespace readfile
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
        private void OpenButton_Click(object sender, RoutedEventArgs e)
        {
            string filePath = txtFilePath.Text;

            if (File.Exists(filePath))
            {
                string fileContent = File.ReadAllText(filePath);
                txtFileContent.Text = fileContent;
            }
            else
            {
                txtFileContent.Text = "File not found";
            }
        }
    }
}
```

### MainWindow.xaml

```xml
<Window x:Class="readfile.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:readfile"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="File Path:" />
            <TextBox x:Name="txtFilePath" Width="200" Margin="0 5" />
            <Button Content="Open" Click="OpenButton_Click" />
            <TextBox x:Name="txtFileContent" Width="300" Height="200" Margin="0 10" IsReadOnly="True" />
        </StackPanel>
    </Grid>
</Window>
```
