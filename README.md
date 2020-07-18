# DrawDottedLine
In xamarin.forms Draw dotted line effect


<html>
<p align="center">
  <img src="https://github.com/dorisoy/DrawDottedLine/blob/master/Screenshot_1.jpg?raw=true" height="1000">
  <img src="https://github.com/dorisoy/DrawDottedLine/blob/master/Screenshot_2.jpg?raw=true" height="1000">
</p>
</html>


### 1. Create custom a ContentView
  ```csharp
  <?xml version="1.0" encoding="UTF-8"?>
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="DCMS.Client.CustomViews.DashedLineView" Padding="0" Margin="0">
</ContentView>
  ```
### 2. using SkiaSharp
  ```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace DCMS.Client.CustomViews
{
    [XamlCompilation(XamlCompilationOptions.Compile)]
    public partial class DashedLineView : ContentView
    {
        public Color LineColor
        {
            get; set;
        } = Color.Black;

        public float DashSize
        {
            get; set;
        } = 20;

        public float WhiteSize
        {
            get; set;
        } = 20;

        public float Phase
        {
            get; set;
        } = 0;

        public DashedLineView()
        {
            Content = new SKCanvasView();
            ((SKCanvasView)Content).PaintSurface += Canvas_PaintSurface;
        }

        private void Canvas_PaintSurface(object sender, SKPaintSurfaceEventArgs args)
        {
            SKImageInfo info = args.Info;
            SKSurface surface = args.Surface;
            SKCanvas canvas = surface.Canvas;

            canvas.Clear();

            SKPaint paint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                Color = LineColor.ToSKColor(),
                StrokeWidth = HeightRequest > 0 ? (float)HeightRequest : (float)WidthRequest,
                StrokeCap = SKStrokeCap.Butt,
                PathEffect = SKPathEffect.CreateDash(new float[] { DashSize, WhiteSize }, Phase)
            };

            SKPath path = new SKPath();
            if (HeightRequest > 0)
            {
                // Horizontal
                path.MoveTo(0, 0);
                path.LineTo(info.Width, 0);
            }
            else
            {
                // Vertikal
                path.MoveTo(0, 0);
                path.LineTo(0, info.Height);
            }

            canvas.DrawPath(path, paint);
        }
    }
}
  ```
### 3. Quickstart
  ```csharp
<custom:DashedLineView HeightRequest="10"
      Grid.Column="1"
      VerticalOptions="Start"
      HorizontalOptions="Fill"
      LineColor="#dddddd"
      DashSize="10"
      WhiteSize="10"
      Margin="0,10,0,0" />
  ```
  
 -  You can do this layout
 
```csharp
  <Grid Grid.Row="4"
        Grid.Column="0"
        Grid.ColumnSpan="2"
        IsVisible="{Binding IsLast}">
      <Grid.RowDefinitions>
          <RowDefinition Height="20" />
      </Grid.RowDefinitions>
      <Grid.ColumnDefinitions>
          <ColumnDefinition Width="10" />
          <ColumnDefinition Width="*" />
          <ColumnDefinition Width="10" />
      </Grid.ColumnDefinitions>
      <BoxView BackgroundColor="#dddddd"
               Grid.Column="0"
               CornerRadius="50"
               VerticalOptions="Center"
               Margin="-10,0,0,0" />
      <custom:DashedLineView HeightRequest="10"
                             Grid.Column="1"
                             VerticalOptions="Start"
                             HorizontalOptions="Fill"
                             LineColor="#dddddd"
                             DashSize="10"
                             WhiteSize="10"
                             Margin="0,10,0,0" />
      <BoxView BackgroundColor="#dddddd"
               Grid.Column="2"
               CornerRadius="50"
               VerticalOptions="Center"
               Margin="0,0,-10,0" />
  </Grid>
  ```
  # Dependencies
- SkiaSharp
- SkiaSharp.Views
- SkiaSharp.Views.Forms
