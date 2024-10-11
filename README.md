# Taiwanese_Correction_frontend

## Project Setup
#### Place this into Taibun server at `C:\Program Files\Taiwanese_Correction\Taiwanese_Correction_v2`

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

### RUN project using Docker
```sh
docker compose up --build -d
```

### Remove all container
```sh
docker compose down
```

### Web Structure

```js
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