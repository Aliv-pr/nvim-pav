# Neovim LSP – Windows + PowerShell

Este README documenta **mi configuración actual de LSP en Neovim**, pensada **exclusivamente para Windows usando PowerShell**. El objetivo es no olvidar:

* qué plugins están instalados
* qué atajos de teclado existen
* cómo instalar y configurar servidores LSP siguiendo este mismo esquema

---

## 🧩 Plugins instalados

### mason.nvim

Repositorio: `williamboman/mason.nvim`

**Rol:**

* Administrador de binarios LSP/DAP/Linters
* Solo instala servidores, **no los configura ni los arranca**

**Configuración actual:**

```lua
{
  "williamboman/mason.nvim",
  config = function()
    require("mason").setup()
  end
}
```

---

### nvim-lspconfig

Repositorio: `neovim/nvim-lspconfig`

**Rol:**

* Configurar y arrancar servidores LSP
* Define cómo Neovim se comunica con cada lenguaje

**Configuración base actual:**

```lua
{
  "neovim/nvim-lspconfig",
  config = function()
    local lspconfig = require("lspconfig")

    lspconfig.lua_ls.setup({
      cmd = { "lua-language-server.cmd" }
    })

    vim.keymap.set('n', 'K', vim.lsp.buf.hover, {})
    vim.keymap.set('n', 'gd', vim.lsp.buf.definition, {})
    vim.keymap.set({ 'n', 'v' }, '<leader>ca', vim.lsp.buf.code_action, {})
  end
}
```

---

## ⌨️ Atajos de teclado LSP

Atajos definidos **manualmente** dentro de la configuración LSP:

| Atajo        | Modo            | Acción                        |
| ------------ | --------------- | ----------------------------- |
| `K`          | Normal          | Mostrar documentación (hover) |
| `gd`         | Normal          | Ir a definición               |
| `<leader>ca` | Normal / Visual | Code actions                  |

> Estos atajos solo funcionan cuando un servidor LSP está correctamente adjunto al buffer.

---

## 🔧 Proceso de instalación de servidores LSP

Este proceso **no usa `mason-lspconfig`**, todo es explícito y controlado manualmente.

### 1️⃣ Instalar el servidor con Mason

Abrir Neovim y ejecutar:

```vim
:Mason
```

Buscar e instalar el servidor deseado.

Ejemplo para Lua:

* `lua-language-server`

---

### 2️⃣ Configurar el servidor con nvim-lspconfig

En Windows **siempre usar el ejecutable `.cmd`** que instala Mason.

#### Ejemplo: Lua (`lua_ls`)

```lua
local lspconfig = require("lspconfig")

lspconfig.lua_ls.setup({
  cmd = { "lua-language-server.cmd" }
})
```

---

### 3️⃣ Verificar que el LSP funciona

Dentro de un archivo del lenguaje correspondiente:

```vim
:LspInfo
```

Debe aparecer algo como:

* `Client: lua_ls`
* `cmd: lua-language-server.cmd`
* `autostart: true`

---

## 🧠 Notas importantes (Windows)

* Mason instala binarios en una ruta interna de Neovim
* **No depende del PATH global de Windows**
* Siempre usar `*.cmd` en `cmd`
* Si un servidor dice que no existe:

  * revisar `:LspInfo`
  * reiniciar Neovim
  * verificar que Mason terminó la instalación

---

## 🛠️ Comandos útiles de diagnóstico

```vim
:LspInfo
:checkhealth lspconfig
:Mason
```

---

## 🎯 Filosofía de esta configuración

* Entorno fijo: **Windows + PowerShell**
* Configuración explícita > magia automática
* Mason solo instala
* LSPConfig decide cómo y cuándo se lanza el servidor

Este README es la referencia principal para futuras configuraciones.

