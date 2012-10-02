ninja.sg
========

Home of the shinobi

## Installing Dependencies

    sudo gem update --system
    sudo gem install jekyll

## Local Development

1. Launch Jekyll

        jekyll --server --url http://0.0.0.0:4000

1. Browse to [http://0.0.0.0:4000](http://0.0.0.0:4000)

## PubSubHubbub Publishing

When publishing a new article, ping the PubSubHubbub hub to push-notify all feed subscribers. Jekyll on GitHub Pages does not support this so use the bookmarklet to manually ping the hub.

[Bookmarklet](http://pubsubhubbub.appspot.com/bookmarklet_config.html)