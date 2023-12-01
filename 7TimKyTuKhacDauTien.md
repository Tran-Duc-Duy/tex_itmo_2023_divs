# Сформируйте контрол, который находит первый различный символ в 2х текстах

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
            <TextBlock Text="Text 1:" />
            <TextBox x:Name="txtText1" Width="200" Margin="0 5" />
            <TextBlock Text="Text 2:" />
            <TextBox x:Name="txtText2" Width="200" Margin="0 5" />
            <Button Content="Compare" Click="CompareButton_Click" />
            <TextBlock x:Name="txtResult" Margin="0 10" />
        </StackPanel>
    </Grid>
</Window>
```

###

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

    }
    private void CompareButton_Click(object sender, RoutedEventArgs e)
    {
        string text1 = txtText1.Text;
        string text2 = txtText2.Text;

        if (!string.IsNullOrEmpty(text1) && !string.IsNullOrEmpty(text2))
        {
            int minLength = Math.Min(text1.Length, text2.Length);

            for (int i = 0; i < minLength; i++)
            {
                if (text1[i] != text2[i])
                {
                    txtResult.Text = $"First differing character at index {i}: '{text1[i]}' vs '{text2[i]}'";
                    return;
                }
            }

            if (text1.Length != text2.Length)
            {
                int differingIndex = minLength;
                char differingChar = text1.Length > text2.Length ? text1[differingIndex] : text2[differingIndex];
                txtResult.Text = $"Texts differ in length. First differing character at index {differingIndex}: '{differingChar}'";
            }
            else
            {
                txtResult.Text = "Texts are identical";
            }
        }
        else
        {
            txtResult.Text = "Please enter both texts";
        }
    }
}
```
