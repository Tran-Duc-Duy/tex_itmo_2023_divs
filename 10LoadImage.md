# Сформируйте контрол, визуализирующий все картинки из папки изображения пользователя

```cs
public partial class MainWindow : UserControl
{
    public MainWindow()
    {
        InitializeComponent();
        LoadImagesFromFolder();
    }

    private void LoadImagesFromFolder()
    {
        string imagesFolderPath = @"D:\duydaily";

        if (Directory.Exists(imagesFolderPath))
        {
            string[] imageFiles = Directory.GetFiles(imagesFolderPath, "*.jpg");

            foreach (string imageFile in imageFiles)
            {
                imageListBox.Items.Add(imageFile);
            }
        }
        else
        {
            MessageBox.Show("Папка изображений не найдена.");
        }
    }
}
```

```xml
<UserControl x:Class="wordFinder.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wordFinder"
        mc:Ignorable="d" Height="450" Width="800">

    <Grid>
        <ListBox x:Name="imageListBox" HorizontalContentAlignment="Stretch" VerticalContentAlignment="Stretch">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <Image Source="{Binding}" Stretch="UniformToFill" />
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </Grid>
</UserControl>
```
