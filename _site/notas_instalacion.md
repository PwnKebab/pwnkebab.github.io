Solucionar problemas:

```sh
sudo bundle update
```
modificar el fichero Gemfile:

```sh
diego@contabo Blog/no-style-please (master *%) » cat Gemfile
# frozen_string_literal: true

source "https://rubygems.org"

gem "kramdown-parser-gfm"
gem "webrick"

gemspec
```

Para ejecutar:

```sh
sudo bundle exec jekyll serve --host=0.0.0.0 --incremental
```

Nos bajamos un syntax highlighter:

    http://jwarby.github.io/jekyll-pygments-themes/languages/java.html

En este caso, he elegido GitHub. Se guaarga en assets/css/github.css

Hay que modificar el post.html (_layouts) para que tenga el siguiente contenido:

```html
---
layout: default
---

{%-include back_link.html-%}
<link rel="stylesheet" href="{{ "/assets/css/github.css" | relative_url }}" />
<article>
  <p class="post-meta">
    <time datetime="{{ page.date }}">{{ page.date | date: site.theme_config.date_format }}</time>
  </p>
  
  <h1>{{ page.title }}</h1>

  {{ content }}
</article>
```

En el _sass/no-style-please.scss se puede configurar el tamaño de la fuente y el ancho del blog:

```scss
.w {
  //max-width: 640px;
  max-width: 1000px;
  margin: 0 auto;
  padding: 4rem 2rem;
}
```

```scss
// lo he cambiado para que cuando estemos en dark mode no salga el bloque en blanco
// campo font-size modificado
code {
  color: black;
  background: white;
  font-size: 14px;
}
```
Ahora se busca que haya una barra que te permita desplazarte en el código de manera horizontal:

```scss
// -------------- THEME SWITCHER -------------- //
@mixin dark-appearance {
  filter: invert(1);
  img {
    filter: invert(1);

    &.ioda { filter: invert(0); }
  }
}

body[a="dark"] { @include dark-appearance; }


@media (prefers-color-scheme: dark) {
  body[a="auto"] { @include dark-appearance; }
}
// -------------------------------------------- //

// bg color is also needed in html in order to
// block body's background propagation
// see: https://stackoverflow.com/a/61265706
html, body { background: white; }

html { height: 100%; }

body {
  color: black;
  font-family: monospace;
  font-size: 16px;
  line-height: 1.4;
  margin: 0;
  min-height: 100%;
  overflow-wrap: break-word;
}

.post-meta { text-align: right; }

h2, h3, h4, h5, h6 { margin-top: 3rem; }

hr { margin: 2rem 0; }

p { margin: 1rem 0; }

li { margin: 0.4rem 0; }

*:target { background: yellow; }

.w {
  //max-width: 640px;
  max-width: 1000px;
  margin: 0 auto;
  padding: 4rem 2rem;
}

hr {
  text-align: center;
  border: 0;

  &:before { content: '/////' }
  &:after { content: attr(data-content) '/////' }
}

table { width: 100%; }

table, th, td {
  border: thin solid black;
  border-collapse: collapse;
  padding: 0.4rem;
}

// lo he cambiado para que cuando estemos en dark mode no salga el bloque en blanco
// campo font-size modificado
code {
  color: black;
  background: white;
  font-size: 14px;
  overflow: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
}

div.highlighter-rouge code {
  display: block;
  overflow-x: auto; // Habilita desplazamiento horizontal
  white-space: pre; // Mantiene los saltos de línea y espacios
  padding: 1rem;
  // Considera añadir un ancho máximo si es necesario
}


blockquote {
  font-style: italic;
  border: thin solid black;
  padding: 1rem;

  p { margin: 0; }
}

img {
  max-width: 100%;
  display: block;
  margin: 0 auto;
}
```