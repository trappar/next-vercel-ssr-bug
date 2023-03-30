# Next Vercel SSR Bug

This is a minimal reproduction repo which demonstrates an issue where PNPM monorepos fail to deploy properly to Vercel when using prebuilt builds.

I believe this is an example case of:
* https://github.com/vercel/vercel/issues/8794
* https://github.com/vercel/next.js/discussions/39432

## Steps to reproduce

1. Clone this repo
2. Change directories to the app foler with `cd app`
3. Setup new Vercel
   1. Run `vercel`.
   2. Answer "Y" to "Set up and deploy..."
   3. Select whatever scope
   4. Select "N" for "Link to existing project?"
   5. Enter whatever app name
   6. Enter "./" for "In which directory is your code located?"
   7. Select "N" for "Want to modify these settings?"
   8. Once you hit the building step, you can cancel it and move on to the next step.
4. Deploy the app to Vercel
    ```
    vercel pull && vercel build && vercel deploy --prebuilt
    ```

## Expected behavior

Upon visiting the application, you should see a page with the text "Hello World".

## Actual behavior

Upon visiting the application, you will receive a 500 error

If you view the logs, you will see the following error:
```
2023-03-30T23:24:34.021Z	undefined	ERROR	Cannot find module 'next/dist/server/next-server.js'
Require stack:
- /var/task/___next_launcher.cjs
2023-03-30T23:24:34.021Z	undefined	ERROR	Did you forget to add it to "dependencies" in `package.json`?
RequestId: 7fc13ce8-5946-4ae4-97c4-223686ad2168 Error: Runtime exited with error: exit status 1
Runtime.ExitError
```

## Notes

This issue does not exist when deploying this exact app using a non-prebuilt deploy. Try running the following in the `app` directory:
```
vercel deploy
```

This deployment will succeed, and will not have the error described above.