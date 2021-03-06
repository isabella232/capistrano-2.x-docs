**Note**: _Please be sure to use Capistrano 2.15.4 or higher when deploying any Rails 4 application, new or upgraded_
## Asset Pipeline
Due to an important change in the format and naming of the assets manifest file that Sprockets generates in Rails 4, when you first precompile the assets for your newly upgraded Rails 4 application, Sprockets will ignore your Rails 3.x manifest and generate a new one. For other Capistrano tasks to work, there must only be one manifest in your production assets directory (`shared/assets` by default).

This should be remedied immediately prior to deploying your updated application for the first time.

Recent versions of Capistrano backup this file to the release directory during each deploy.  Please confirm that your current Rails 3.x release directory contains an `assets_manifest.yml` in its root.  If it does, then it is safe to remove the `manifest.yml` file in your assets directory.  If it does not exist, you should move your `manifest.yml` file to your current release's root directory and name it `assets_manifest.yml`.  Should you need to roll back your Rails 4 deploy, Capistrano will restore it for you.   

***Note:*** If you are arriving on this page because Capistrano detected two manifest files and told you to check it out, your assets directory contains both your Rails 3.x manifest and your Rails 4 manifest. Follow the above instructions for your Rails 3.x manifest (`manifest.yml`) and ignore the Rails 4 manifest, which is a `.json` file with a random filename (`manifest-RANDOM.json`).