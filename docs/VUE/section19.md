## Section 19 VUE - Deploy to Production

### 1. deploy with Vercel

finding a server to delopy the app, eg: Heroku, [Vercel](https://vercel.com/).

```
npm run build
```

under `/dist` folder, that all we need to deploy to prod.

upload with Vercel:

```
npm i -g vercel
```

login:

```
vercel login
```

❌: blocked by the below error:

```
internal/modules/cjs/loader.js:892
  throw err;
  ^

Error: Cannot find module 'stream/web'
Require stack:
```

### 2. vite doc - vercel

https://vitejs.dev/guide/static-deploy#vercel
