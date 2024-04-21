```JS
export default defineConfig({
  root: './src', // папка где,лежит index.html
  publicDir: '../public', // папка где лежат статические файлы
  envDir: '../', // The directory from which `.env` files are loaded. Can be an absolute path, or a path relative to the project root.
  build: { //
	  //target // Browser compatibility target for the final bundle. The default value is a Vite special value, `'modules'`, which targets browsers with [native ES Modules](https://caniuse.com/es6-module), [native ESM dynamic import](https://caniuse.com/es6-module-dynamic-import), and [`import.meta`](https://caniuse.com/mdn-javascript_operators_import_meta) support. Vite will replace `'modules'` to `['es2020', 'edge88', 'firefox78', 'chrome87', 'safari14']`
    outDir: '../dist' // Specify the output directory
  },
  define: {
    // enable hydration mismatch details in production build
    __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: 'true' // Define global constant replacements. Entries will be defined as globals during dev and statically replaced during build.
  },
  server: {
    host: '0.0.0.0', // Specify which IP addresses the server should listen on. Set this to `0.0.0.0` or `true` to listen on all addresses, including LAN and public addresses.
    port: 3000 // Specify server port. Note if the port is already being used, Vite will automatically try the next available port so this may not be the actual port the server ends up listening on.
  },
  plugins: [ // plagins
    vue(),
    createSvgIconsPlugin({
      iconDirs: [resolve(process.cwd(), './public/images/svg')],
      symbolId: 'icon-[dir]-[name]'
    }),
    paths({ root: '../' })
  ]
});
```