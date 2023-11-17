# win2d를 이용한 배너와 c#을 이용한 곱셈 계산기
## C#을 이용한 곱셈 계산기
### 주요 코드
```xaml
<StackPanel Orientation="Vertical" HorizontalAlignment="Center" VerticalAlignment="Center">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBox x:Name="tb1" TextAlignment="Center"></TextBox>
            <TextBox x:Name="tb2" TextAlignment="Center"></TextBox>
        </StackPanel>
        <Button x:Name="bt1" Click="bt1_Click" Width="120">Mul</Button>
        <TextBox x:Name="tb3" TextAlignment="Center"></TextBox>
    </StackPanel>
```
```c#
private void bt1_Click(object sender, RoutedEventArgs e)
        {
            double a, b, c;
            double.TryParse(tb1.Text, out a);
            double.TryParse(tb2.Text, out b);
            c = a * b;
            tb3.Text=c.ToString();
        }
```
