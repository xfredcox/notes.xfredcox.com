# Static Hosting on S3 

The simplest, most economic, lightning fast way of hosting SPAs. A few notes on setup, advanced features and limitations.

Coming from a python background, its very rewarding to be able to deploy apps as static files.

### Grunt Deployment

This is the grunt task I use for immediate staging and/or production deployment:

``javascript
// Config info goes in an untracked .json file
aws: grunt.file.readJSON('aws-keys.json'),
// API Config
aws_s3: {
      options: {
          accessKeyId: '<%= aws.AWSAccessKeyId %>',
          secretAccessKey: '<%= aws.AWSSecretKey %>',
          region: '<%= aws.AWSRegion %>',
          uploadConcurrency: 5,
          downloadConcurrency: 5
      },
      staging: {
          options: {
              bucket: '<%= aws.StagingDomain %>',
              differential: true
          },
          files: [
              {expand: true, cwd: '<%= yeoman.dist %>', src: ['**'], dest: ''},
          ]
      },
      deploying: {
          options: {
              bucket: '<%= aws.ProductionDomain %>',
              differential: true
          },
          files: [
             {expand: true, cwd: '<%= yeoman.dist %>', src: ['**'], dest: ''},
          ]
      }
   }
``

The task requires this [Grunt AWS library](https://www.npmjs.com/package/grunt-aws-s3) and setting the appropriate bucket policy permission setup.
### Routing

You won't get all the features of a web server, but S3 does have some [routing functionality](https://docs.aws.amazon.com/AmazonS3/latest/dev/HowDoIWebsiteConfiguration.html) that might serve your use case. Recently, I've been re-routing requests from search engines to pre-rendered content buckets. For example

``html
<RoutingRules>
    <RoutingRule>
        <Condition>
            <KeyPrefixEquals>?_escaped_fragment_=</KeyPrefixEquals>
        </Condition>
        <Redirect>
            <HostName>www.xfredcox.com</HostName>
            <ReplaceKeyPrefixWith>snapshots</ReplaceKeyPrefixWith>
            <HttpRedirectCode>302</HttpRedirectCode>
        </Redirect>
    </RoutingRule>
</RoutingRules>
``

So requests containing the escaped fragment in the query string get **temporarily** redirected to the /snapshots/* subdirectory (where we stash the pre-rendered content).

### Limitations

Even modest projects may have requirements that will demand long-running or ad hoc server tasks. For me, these have included Stripe integration, complex web server routing and search engine friendly rendering (SEO). There are workarounds where you can still host the app statically and run tasks separately. We'll talk about a few of these in future posts.