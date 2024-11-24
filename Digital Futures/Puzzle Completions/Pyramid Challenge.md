```cs
var rows = 10;
var width = 8;

// Outer loop controls the operations for each row
for (int row = 1; row <= rows; row++)
{
    // Inner loop controls the operations for each star in the row
    for (int star = 1; star <= row; star++)
    {
        // If the star is the first star in the row, we need to pad it to the left based on the row number
        if (star == 1)
        {
            Console.Write("*".PadLeft(rows*width + width - row*width));
        }
        // The rest of the stars in the row are padded to the left based on the width
        else
        {
            Console.Write("*".PadLeft(2*width));
        }
    }
    Console.WriteLine("");
}

```

