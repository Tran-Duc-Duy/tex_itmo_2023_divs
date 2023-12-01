# Сформируйте контрол, визуализирующий изображение по переданной ссылке

```cs
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.IO;
using System.Runtime.CompilerServices;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media.Imaging;

namespace wordFinder
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : UserControl
    {
        public static readonly DependencyProperty ImageUrlProperty =
        DependencyProperty.Register("ImageUrl", typeof(string), typeof(MainWindow));

        public string ImageUrl
        {
            get { return (string)GetValue(ImageUrlProperty); }
            set { SetValue(ImageUrlProperty, value); }
        }

        public MainWindow()
        {
            InitializeComponent();
            Loaded += MainWindow_Loaded;
        }

        private void MainWindow_Loaded(object sender, RoutedEventArgs e)
        {
            if (!string.IsNullOrEmpty(ImageUrl))
            {
                DisplayImageFromUrl(ImageUrl);
            }
        }

        private void DisplayImageFromUrl(string imageUrl)
        {
            try
            {
                Uri uri = new Uri(imageUrl);
                BitmapImage image = new BitmapImage(uri);
                imageControl.Source = image;
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Failed to display image: {ex.Message}");
            }
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            ImageUrl = imageUrlTextBox.Text;
            DisplayImageFromUrl(ImageUrl);
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
        <TextBox x:Name="imageUrlTextBox" VerticalAlignment="Top" HorizontalAlignment="Left" Width="200" Margin="10"/>
        <Button Content="Display Image" VerticalAlignment="Top" HorizontalAlignment="Left" Width="100" Height="25" Margin="220,10,0,0" Click="Button_Click"/>
        <Image x:Name="imageControl" VerticalAlignment="Top" HorizontalAlignment="Left" Width="400" Height="300" Margin="10,40,0,0"/>
    </Grid>
</UserControl>

```
