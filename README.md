# STAY — sitio web (stay.com.pa)

Sitio de una sola página para **STAY**, edificio de lujo de renta en Ciudad de Panamá
(Urbania Developer & Emporium Developers). Estático, sin build.

## Estructura
- `index.html` — el sitio (rutas relativas a `assets/`).
- `assets/web/` — imágenes (webp) · `assets/logos/` — logos.
- `_headers` — reglas de caché para Cloudflare Pages.

## Deploy automático (Cloudflare Pages)

**Opción A — Git integration (recomendada, sin secrets):**
1. Cloudflare Dashboard → Workers & Pages → Create → Pages → **Connect to Git**.
2. Selecciona este repo. Framework preset: **None**. Build command: *(vacío)*.
   Build output directory: **`/`** (raíz).
3. Deploy. La rama `main` = producción; otras ramas (`taller`) = previews.
4. Custom domains → agrega **stay.com.pa**.
→ Cada `git push` despliega automáticamente.

**Opción B — GitHub Actions** (si prefieres, como en urbania-site-redesign):
crea `.github/workflows/deploy.yml` (requiere un token con scope `workflow`) con:
```yaml
name: Deploy Stay
on: { push: {}, workflow_dispatch: {} }
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          command: pages deploy . --project-name stay-web --branch ${{ github.ref_name }}
```
y agrega los secrets `CLOUDFLARE_API_TOKEN` y `CLOUDFLARE_ACCOUNT_ID`.

## Notas
- Imágenes comprimidas para web; para máxima calidad reemplazar las de `assets/web/`
  por las full-res (fuente: `STAY Presentacion 2026.pdf`, mismos nombres).
- Pendiente conectar: formulario "Agendar asesoría" y link "Descargar brochure".
- Solo se usaron los renders de la presentación.
