## Installing Dependencies

    gem update --system
    gem install jekyll
    gem install bundler
    bundle install

#### Windows Compatibility

Use Ruby 2.0.0-p645 and DevKit mingw64-32-4.7.2.

## Local Development

1. Run Jekyll

        bundle exec jekyll serve --watch

1. Visit [http://localhost:4000](http://localhost:4000)

## PubSubHubbub Publishing

When publishing a new article, ping the PubSubHubbub hub to push-notify all feed subscribers. Jekyll on GitHub Pages does not support this so use the bookmarklet to manually ping the hub.

[Bookmarklet](http://pubsubhubbub.appspot.com/bookmarklet_config.html)
