---
output:
  html_document:
    code_folding: "hide"
runtime: shiny
---

```{r echo=FALSE, results='hide',message=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(shiny)
library(rmdexamples)
options(shiny.deprecation.messages=FALSE)
```

```{r}

shinyApp( 
ui <- shinyUI(fluidPage(

    title = "BMI Calculator",

     
      fluidRow(
        
        column(10, 
               h1("BMI Calculator",
                  style = "color: red")), #ff3f34
       column(1, 
               img(height = 70, 
                   width = 100, 
      src = "https://1.bp.blogspot.com/-m3ZpuTW79SE/XVvqTPad5sI/AAAAAAAAFbw/1k_h1LUzrSYeOTsQyJ10vleUCP1JjcRpQCLcBGAs/s1600/realr.jpg")
        ) #logo
        
      ),

    hr(),
    
column(12, div(h4("Aplikasi ini menggunakan rumus dasar Indeks Massa Tubuh (BMI) dengan variabel 
tinggi badan dalam inci dan berat dalam pound 
Untuk  formula yang lebih maju akan memperhitungkan faktor-faktor seperti jenis kelamin, lingkar dada dan pinggang
      "), style = "color:green", align = "left")),
 
column(12, div(h4("Baris pertama akan berubah dinamis mengkonversi inci ke meter. Baris kedua akan mengkonversi 
berat ke kg. Hasil BMI akan tertulis di bawahnya"), style = "color:green",align = "left")),
 
 hr(),

    fluidRow(
        column(4, offset = 1,
               wellPanel(sliderInput("ht", "Tinggi badan dlm inci:", min = 1,
                                     max = 108, value = 65)),
               wellPanel(sliderInput("wt", "Berat badan dlm pound:", min = 1,
                                      max = 700, value = 150))
        ),

        column(6, offset = 1,
               div(textOutput("htText"), style = "color:blue;font-size:16pt"),
               div(textOutput("wtText"), style = "color:blue;font-size:16pt"),
               br(),
               div(strong("BMI: ", style = "color:blue;font-size:17pt")),
               div(strong(textOutput("bmi"),
                          style = "color:red; font-size:40pt")),
               div(strong(textOutput("bmi.class"), style = "font-size:20pt"))
        )
    ),

 hr(),

    h4("Cara penggunaan BMI:"),
    column(12, div(h4("1.Geser panel masukkan tinggi badan dlm inci"), style = "color:green")),
    column(12, div(h4("2.Masukkan berat badan dalam pound"), style = "color:green"))
)),

# This is the server logic for a Shiny web application.
# You can find out more about building applications with Shiny here:
#
# http://shiny.rstudio.com
#

server <- shinyServer(function(input, output) {

    htInput <- reactive({input$ht})

    wtInput <- reactive({input$wt})

   calcBMI <- reactive({round(703 * (wtInput() / htInput() ^ 2), 2)})

    classBMI <- reactive({
        if (calcBMI() < 18.5) {
            return("Underweight")
        } else {
            if (calcBMI() < 25) {
                return("Healthy Weight")
            } else {
                if (calcBMI() < 30) {
                    return("Overweight")
                } else {
                    return("Obese")
                }
            }
        }
    })

    output$htText <- renderText({
        # Print the height from the slider input in feet and inches (meters)
        paste0("Height: ", floor(htInput() / 12), "'", htInput() %% 12, "\"",
               " (", round(htInput() * 0.0254, 2), "m)")
    })

    output$wtText <- renderText({
        # Print the weight from the numeric input in pounds (kilograms)
        paste0("Weight: ", wtInput(), "lbs (", round(wtInput() * 0.453592, 2),
               "kg)")
    })

    output$bmi <- renderText({
        # Calculate BMI and print it
        calcBMI()
    })

    output$bmi.class <- renderText({
        classBMI()
    })

}))
```

