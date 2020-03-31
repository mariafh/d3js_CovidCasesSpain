# COVID-19 in Spain:
# Including background color depending on affected number of cases.

 - Circles are placed in every region with   
people affected by COVID-19. 
 - Scale pin radius is based on affected number.
 - Background color of communities is based on affected number.


![Map](/content/map.png)

This code is based on:

- Lemoncode / d3js-typescript-examples  
[https://github.com/Lemoncode/d3js-typescript-examples/tree/master/02-maps/02-pin-location-scale](https://github.com/Lemoncode/d3js-typescript-examples/tree/master/02-maps/02-pin-location-scale)
- mariafh / d3js_Visualization_Mandatory 
https://github.com/mariafh/d3js_Visualization_Mandatory

# Steps

## Installation and Running

- Execute _npm install_.
```bash
npm install
```
- Execute _npm start_.
```bash
npm start
```
## Coding

- Changing background color of communities:
 - Delete fill attribute in _map.css_ _.country_
_map.css_
.country {
  stroke-width: 1;
  stroke: #2f4858;
}
 - Create a scaleThreshold of colors depending on affected cases (domain)
_.src/index.ts_
  ```typescript
var colors = d3 
  .scaleThreshold<number, string>()
  .domain([1, 10, 40, 100, 300, 500, 700, 1000, 2000, 6000, 7000, 8000, 10000, 12000])
  .range(["#edf7c0", "#e4f5b0", "#daf4a1", "#cef393", "#c0f285", "#b2ec79", "#a3e76d", "#93e162", "#80d555", "#6dc848", "#59bc3b", "#43b02e"]);
  ```
 - Change the color depending on the community
_.src/index.ts_
  ```typescript
  const getColorByAffectedCases = (name: string) => {
    const item = data.find(
      item => item.name == name
    );
    return  item ? colors(item.value) : colors(0);
  }
  ```
 - Change svg "path", filling the communities depending on the _spain.json_ field which means 'Community Name' => ["properties"]["NAME_1"]
    ```typescript
    svg
    .selectAll("path")
    .data(geojson["features"])
    .enter()
    .append("path")
    .attr("class", "country")
    .style("fill", (d) => getColorByAffectedCases(d["properties"]["NAME_1"]))
    .attr("d", geoPath as any)
    .merge(svg.selectAll("path") as any)
    .transition()
    .duration(400)
    .attr("d", geoPath as any)
    .style("fill", (d) => getColorByAffectedCases(d["properties"]["NAME_1"]));
    ```