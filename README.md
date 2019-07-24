# Cartographie intéractive des résultats à Paris lors des élections présidentielles 2017

## Shiny App

Le but serait de créer une Shiny App reprenant l'ensemble des cartes que j'ai réalisées durant mon mémoire. Celles-ci seraient présentées par thème, par exemple tous les résultats électoraux sur une même carte intéractive avec la possibilité de sélectionner la couche désirée.

## Leaflet

Les cartes seront réalisées dans R grâce à la librairie [Leaflet](https://rstudio.github.io/leaflet/) de RStudio.

Pour le moment j'ai un code basique reprenant simplement le fond de carte intéractif que je désirais, le zoom défini sur le lieu désiré et la possibilité de revenir directement au point de départ à savoir Paris intra-muros :
```R
  output$mymap <- renderLeaflet({
    # Lat Long de Paris et zoom sur paris intra-muros
    leaflet() %>% setView( 2.3488,48.8580, zoom = 12) %>%
      addProviderTiles(providers$CartoDB.Positron,
                       options = providerTileOptions(noWrap = TRUE) 
      ) %>%
      addMarkers(lng = 2.349902, lat = 48.852966, popup = "Notre Dame") %>%
      addMarkers(data = points())
  })
}
```

Pour la suite il faudrait que je réalise un merge entre mes données électorales / sociales en CSV et mon Shapefile. Mais également que j'arrive à placer le shapefile de manière visible sur la carte. (Le Shapefile des bureaux de vote)
