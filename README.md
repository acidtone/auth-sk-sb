# SvelteKit Auth with Supabase
This is an upgraded version of these Huntabyte tutorials:
1. [SvelteKit Authentication with Supabase](https://www.youtube.com/watch?v=lSm0GNnh-0I)
2. [Protect SvelteKit Routes with Hooks](https://www.youtube.com/watch?v=K1Tya6ovVOI)

## Problem
This tutorial uses a beta version of SK and includes more dependencies than it needs to. 

The goal of this project is to provide an auth template project that:
- Uses Supabase for authentication
- Contains no CSS framework dependencies
- Has no linter, prettier, etc
- Has a sample protected route

Typescript has been included since Huntabyte's code is TS-based but a JS version might be a fallback.

## The plan
Go through the tutorials above and copy paste the relevant code into a fresh SK project and make modifications as needed.

## Session notes
- Cloned the [final code](https://github.com/huntabyte/sk-supabase-auth/tree/final-code) from the first tutorial
- Followed tutorial [SvelteKit Authentication with Supabase](https://www.youtube.com/watch?v=lSm0GNnh-0I):
    1. Set up new Supabase project
        - Setup `.env` with project URL and anon key
    2. Install libraries
        - `npm install -D @supabase/supabase-js`
        - `npm install -D @supabase/auth-helpers-sveltekit`
    3. Create `$lib/supabase.ts`
        - Copy/pasted [this code](https://github.com/huntabyte/sk-supabase-auth/tree/final-code/src/lib)
    4. Create `/src/hooks.server.ts`
        - Copy/pasted [this code](https://github.com/huntabyte/sk-supabase-auth/blob/final-code/src/hooks.server.ts)
    5. Create `/src/hooks.client.ts`
        - Copy/pasted [this code](https://github.com/huntabyte/sk-supabase-auth/blob/final-code/src/hooks.client.ts)
    6. Create `/src/routes/+layout.svelte`
        - Copy/pasted [this code](https://github.com/huntabyte/sk-supabase-auth/blob/final-code/src/routes/%2Blayout.svelte)
        - Removed import of `../app.postcss`
    7. Create `/src/routes/+layout.server.ts`
        - copy.pasted [this code](https://github.com/huntabyte/sk-supabase-auth/blob/final-code/src/routes/%2Blayout.server.ts)
    8. Add types to `/src/app.d.ts`
        - copy/pasted the following lines from [this code](https://github.com/huntabyte/sk-supabase-auth/blob/final-code/src/app.d.ts):
            - Lines 5 and 6
            - Lines 11-16
        - There are differences between beta file and fresh template file.
    9. Booted server to see if there were errors
        - Uh oh, looks like there were [changes between 0.8 and 0.9](https://supabase.com/docs/guides/auth/auth-helpers/sveltekit#migration) in `auth-helpers-sveltekit`
        - There seems to be substantial difference between the Supabase documentation and how Huntabyte wrote his code. Specifically, SB uses `+layout.js` instead of `+layout.server.js`. I'm going to go with Supabase code and try to refactor Huntabyte's as I go through the rest of the tutorial. That's _IF_ I can get the app to run in the first place.
        - Got it working! Ended up following the setup instructions in the [`auth-helpers-sveltekit` docs](https://supabase.com/docs/guides/auth/auth-helpers/sveltekit)
        - Is there a reason to follow the Huntabyte tutorial? Yes (for now) since I would really like to get the email/password functionality working. The Supabase examples use magic links.
    10. Ignoring the tutorial and just refactoring the routes for register, login and logout:
        - `/` - copy and pasted this code but commented out the following until I figure out where to get `.auth.signOut()`:
            - Line 3
            - Lines 9-12 
        - `/register`, `/login` and `/logout` - everything basically just worked once I changed the locals variable from `sb` to `supabase`.
        - Got the `use:enhance` code working. The supabase client is now passed to the page from the `+layout.js` `load` function: `data.supabase.auth.signOut()`
- Next step: Set up a protected route using hooks...