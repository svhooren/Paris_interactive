<p align="center">
	    <h1 align="center">Cartographie intéractive des résultats à Paris lors des élections présidentielles 2017 avec R Leaflet</h1>
	    <img src="/Images/0.png">
	    <br><br><br>
	</p>

## Shiny App

Le but serait de créer une Shiny App reprenant l'ensemble des cartes que j'ai réalisées durant mon mémoire. Celles-ci seraient présentées par thème, par exemple tous les résultats électoraux sur une même carte intéractive avec la possibilité de sélectionner la couche désirée.

## Leaflet

Les cartes seront réalisées dans R grâce à la librairie [Leaflet](https://rstudio.github.io/leaflet/) de RStudio.

Pour le moment j'ai un code basique reprenant simplement le fond de carte intéractif que je désirais, le zoom défini sur le lieu désiré et la possibilité de revenir directement au point de départ à savoir Paris intra-muros. J'ai également introduit les shapefile des bureaux de votes lors des élections présidentielles 2017 et trier mes données électorales. Une légende pour pouvoir observer la couche désirée sur le fond de carte souhaité.

```R
library(shiny)
library(rgdal)
library(leaflet)

# Define UI for application
ui <- fluidPage(
   
   # Application title
   titlePanel("Cartographie de Paris lors des élections présidentielles 2017 à l'échelle des bureaux de vote."),
   
   ui <- fluidPage(
     leafletOutput("mymap"),
     p(),
     actionButton("recalc", "Paris Intra-muros")
   ))

# Define server logic required to draw a histogram
server <- function(input, output, session) {
  
  points <- eventReactive(input$recalc, {
    cbind(rnorm(40) * 2 + 13, rnorm(40) + 48)
  }, ignoreNULL = FALSE)
  
  # Description des points 
  description <- paste(sep = "<br/>",
                       "<b>Notre Dame</b>",
                       "Paris, France",
                       "<img src='https://fr.wikipedia.org/wiki/Cathédrale_Notre-Dame_de_Paris#/media/Fichier:Notre_Dame_de_Paris_2013-07-24.jpg' height='40' width='50'>",
                       "<a href=https://www.notredamedeparis.fr/>Voir le site </a>")
  
  output$mymap <- renderLeaflet({
    # Lat Long de Paris et zoom sur paris intra-muros
    leaflet() %>% setView( 2.3488,48.8580, zoom = 12) %>%
      # Base groups
      addTiles(group = "OSM (default)") %>%
      addProviderTiles(providers$CartoDB.Positron,
                       options = providerTileOptions(noWrap = TRUE) 
      ) %>%
      # Zoom and point
      addMarkers(lng = 2.349902, lat = 48.852966, popup = description) %>%
      addMarkers(data = points())  %>%
      # Overlay groups
      addPolygons(data=secteurs,opacity = 20,weight=1.2,col = 'grey', group = "Bureaux de vote 2017") %>%
      addPolygons(data=Fillon,opacity = 20,weight=1.2,col = 'white', group = "Fillon") %>%
      addLayersControl(
        baseGroups = c("OSM", "Positron"),
        overlayGroups = c("Bureaux de vote 2017", "Fillon"),
        options = layersControlOptions(collapsed = FALSE)
    )

  })
}

# Run the application 
shinyApp(ui = ui, server = server)

```
