# Сформируйте контрол, который в месте клика рисует красную точку

### MainWindow.xaml:

```xaml
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Draw Red Dot" Height="400" Width="400"
        MouseLeftButtonDown="Canvas_MouseLeftButtonDown">
    <Grid>
        <Canvas x:Name="canvas" Background="White" MouseLeftButtonDown="Canvas_MouseLeftButtonDown" />
    </Grid>
</Window>
```

MainWindow.xaml.cs:

```csharp
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Shapes;

namespace WpfApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Canvas_MouseLeftButtonDown(object sender, MouseButtonEventArgs e)
        {
            Point clickPosition = e.GetPosition(canvas);

            Ellipse redDot = new Ellipse
            {
                Width = 10,
                Height = 10,
                Fill = Brushes.Red
            };

            Canvas.SetLeft(redDot, clickPosition.X - redDot.Width / 2);
            Canvas.SetTop(redDot, clickPosition.Y - redDot.Height / 2);

            canvas.Children.Add(redDot);
        }
    }
}
```
