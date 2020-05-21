# Deployment!

## You've built (at least) four web apps and a CLI app during your tenure as a student... and you want to share them with the world!  Let's learn how!

_Note:_ remember that your CLI project was called "CLI Data Gem"... while it's possible, and might even be fun, to convert it into a web app, this author's recommendation would be that instead, you [publish it as a gem], just for practice!

## Outline of today's study group:

1. High-level discussion using this README as a guide
2. Deploy an [existing Rails app] to Heroku
3. Deploy an [existing React app] to Netlify
4. ..?..
5. If we have time, we could run breakouts or have folks try to get their sites up and assist with any bugs that pop up

## Deploying to the web

There are myriad choices for deploying websites to the world wide web.  (I had to look up "myriad" to make sure it means "lots", which is does.)  We are going to focus today on two options, [Heroku] and [Netlify].  These are fast, free, and easy to use with the technologies we've learned, which is why they're the platforms we're going to look at.

### High-level steps:

- Both Heroku and Netlify allow you to link a GitHub repository directly to their site.
- Once linked, click 'deploy'.

Um, is it that easy?  Really?... OK, maybe there could be some bugs and other considerations here and there, but pretty much, that's it!  

## Other considerations?

### Database

Sqlite is not going to work with Heroku, and is generally not recommended for production.  That's not to say it _can't_ be used in production -- it can! -- but it has limitations around scaling and concurrency that make it a less-than-stellar option.  Enter [Postgres].  Postgres is another option for a SQL database that is more reliable for a web app and plays nice with Heroku.  If you built your Rails app from start with Postgres, you're pretty much all set already.  If you've built a Rails app with Sqlite, and you'd like to deploy it to Heroku, you still can!  You don't even need to change your development or test databases, just a quick configuration change to Postgres for production.  The basic steps:

1. Move `gem sqlite3` into your development/test groups in your Gemfile.
2. Add `gem pg` to the production group of your Gemfile.
3. `bundle install`
4. Change your `database.yml` production config to something like this:

```yml
production:
  adapter: postgresql
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000
  database: db/production
```
...and violà!

Note that to run test and development PG databases on your local environment, you'll need to actually download [Postgres] onto your machine and make sure it's running.  We'll talk a bit more about this during lecture.

### URLs for AJAX calls (CORS config)

On the front end, you'll need to update your base URL anywhere you make an AJAX call.  This could be in async action creators, React components, utility classes or functions, or even in plain ol' Vanilla JS.

### Auth

If you're using auth, you'll need to ensure you have the proper configuration set up before deployment.  For Rails, if you're using Rails session and a separate frontend, you'll need to white-list your fronted domain.  For other auth tools, you may need to check documentation to see what needs tweaking.

### API keys

If you're using an external API, you may need to use a key that needs to remain hidden.  Read through your deployment platform's documentation on this to find the right solution for your build.

[publish it as a gem]:https://rubygems.org
[Heroku]:https://www.heroku.com/
[Netlify]:https://www.netlify.com/
[Postgres]:https://www.postgresql.org/
[existing Rails app]:
[existing React app]:
