# Taiwanese_Correction_frontend

## Project Setup
#### Place clone this repo into Taibun server `140.116.153.134` at `C:\Program Files\Taiwanese_Correction\Taiwanese_Correction_v2`

### RUN project using Docker
```sh
docker compose up --build -d
```

### Remove all container
```sh
docker compose down
```

### Web Structure

```
<HomePage>
  <NavigationBar>
    <OffCanvasNavBar/>
  </NavigationBar>
  
  <CorrectionMainWidget>
    <FilterWindow />
    <StudentTable>
      <CorrectionTable />
    </StudentTable>
  </CorrectionMainWidget>
</HomePage>
```