# primerejersicio
un ejemplo de como empiezo
// index.jsconst pelis = require('./pelis');
const { program } = require('commander');

program
  .option('--sort <propiedad>', 'Ordenar las películas por la propiedad especificada')
  .option('--search <texto>', 'Buscar películas por texto en el título')
  .option('--tag <nombreDelTag>', 'Filtrar películas por el tag especificado');

program.parse(process.argv);

const options = program.opts();

if (Object.keys(options).length === 0) {
  // Sin parámetros, listar todas las películas
  const todasLasPeliculas = pelis.listarPeliculas();
  console.table(todasLasPeliculas);
} else if (options.sort) {
  // Ordenar películas
  const peliculasOrdenadas = pelis.ordenarPeliculas(options.sort);
  console.table(peliculasOrdenadas);
} else if (options.search) {
  // Buscar películas
  const peliculasEncontradas = pelis.buscarPeliculas(options.search);
  console.table(peliculasEncontradas);
} else if (options.tag) {
  // Filtrar por tag
  const peliculasPorTag = pelis.filtrarPorTag(options.tag);
  console.table(peliculasPorTag);
}

//segundo archivo
//pelis.js
const fs = require('node:fs');
const path = require('node:path');

function leerPeliculas() {
  try {
    const data = fs.readFileSync(path.join(__dirname, 'pelis.json'), 'utf8');
    return JSON.parse(data);
  } catch (error) {
    console.error('Error al leer el archivo pelis.json:', error);
    return [];
  }
}

function listarPeliculas() {
  return leerPeliculas();
}

function ordenarPeliculas(propiedad) {
  const peliculas = leerPeliculas();
  return peliculas.sort((a, b) => {
    const valorA = typeof a[propiedad] === 'string' ? a[propiedad].toLowerCase() : a[propiedad];
    const valorB = typeof b[propiedad] === 'string' ? b[propiedad].toLowerCase() : b[propiedad];

    if (valorA < valorB) {
      return -1;
    }
    if (valorA > valorB) {
      return 1;
    }
    return 0;
  });
}

function buscarPeliculas(texto) {
  const peliculas = leerPeliculas();
  const textoLower = texto.toLowerCase();
  return peliculas.filter(pelicula =>
    pelicula.title.toLowerCase().includes(textoLower)
  );
}

function filtrarPorTag(tag) {
  const peliculas = leerPeliculas();
  const tagLower = tag.toLowerCase();
  return peliculas.filter(pelicula =>
    pelicula.tags.map(t => t.toLowerCase()).includes(tagLower)
  );
}

module.exports = {
  listarPeliculas,
  ordenarPeliculas,
  buscarPeliculas,
  filtrarPorTag,
};

//tercer archivo
//pelis.json
[
    {
      "title": "El Señor de los Anillos: La Comunidad del Anillo",
      "rating": 5,
      "tags": ["fantasía", "aventura", "épica"]
    },
    {
      "title": "Harry Potter y la Piedra Filosofal",
      "rating": 4,
      "tags": ["fantasía", "aventura", "magia", "favorita"]
    },
    {
      "title": "Pulp Fiction",
      "rating": 5,
      "tags": ["crimen", "drama", "clásico"]
    },
    {
      "title": "El Castillo Ambulante",
      "rating": 4.5,
      "tags": ["animación", "fantasía", "aventura"]
    },
    {
      "title": "Matrix",
      "rating": 4,
      "tags": ["ciencia ficción", "acción", "clásico"]
    },
    {
      "title": "Interestelar",
      "rating": 4.8,
      "tags": ["ciencia ficción", "drama", "aventura"]
    },
    {
      "title": "La La Land",
      "rating": 3.5,
      "tags": ["musical", "drama", "romance"]
    }
  ]
