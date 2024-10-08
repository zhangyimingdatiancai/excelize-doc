# Grafico del contorno {#contour}

Ad esempio, aggiungi un grafico di contorno come questo:

<p align="center"><img width="770" src="../images/contour_chart.png" alt="creare un grafico di contorno con Excelize utilizzando Go"></p>

```go
package main

import (
    "fmt"

    "github.com/xuri/excelize/v2"
)

func main() {
    f := excelize.NewFile()
    defer func() {
        if err := f.Close(); err != nil {
            fmt.Println(err)
        }
    }()
    if err := f.SetSheetName("Sheet1", "Foglio1"); err != nil {
        fmt.Println(err)
        return
    }
    for idx, row := range [][]interface{}{
        {nil, "Mela", "Arancia", "Pera"},
        {"Piccolo", 2, 3, 3},
        {"Normale", 5, 2, 4},
        {"Grande", 6, 7, 8},
    } {
        cell, err := excelize.CoordinatesToCellName(1, idx+1)
        if err != nil {
            fmt.Println(err)
            return
        }
        if err := f.SetSheetRow("Foglio1", cell, &row); err != nil {
            fmt.Println(err)
            return
        }
    }
    if err := f.AddChart("Foglio1", "E1", &excelize.Chart{
        Type: excelize.Contour,
        Series: []excelize.ChartSeries{
            {
                Name:       "Foglio1!$A$2",
                Categories: "Foglio1!$B$1:$D$1",
                Values:     "Foglio1!$B$2:$D$2",
            },
            {
                Name:       "Foglio1!$A$3",
                Categories: "Foglio1!$B$1:$D$1",
                Values:     "Foglio1!$B$3:$D$3",
            },
            {
                Name:       "Foglio1!$A$4",
                Categories: "Foglio1!$B$1:$D$1",
                Values:     "Foglio1!$B$4:$D$4",
            },
        },
        Format: excelize.GraphicOptions{
            OffsetX: 15,
            OffsetY: 10,
        },
        Legend: excelize.ChartLegend{
            Position: "left",
        },
        Title: []excelize.RichTextRun{
            {
                Text: "Grafico del contorno",
            },
        },
    }); err != nil {
        fmt.Println(err)
        return
    }
    // Salva cartella di lavoro
    if err := f.SaveAs("Cartel1.xlsx"); err != nil {
        fmt.Println(err)
    }
}
```
