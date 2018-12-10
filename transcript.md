Hello, my name is Dima and today I will tell you about webpack.

It's main purpose is to put your JS, CSS, HTML and other web files into however many bundles you want.

At a glance, it just allows to:
not use immediately invoked functions without pollution of global scope,
and have some nice highlights of modules you import in modern IDEs.

But actually, it's possibilities are much larger.

It can:
convert your code. For example from TypeScript to JavaScript,
preprocess styles,
bundle all your scripts into one or more files,
minify your scripts, styles and HTML so they will load faster.

So actually do all build steps

Let's dive into webpack.


Webpack is mostly about it's configuration.
It works nice out of the box without any configuration,
but it works best when configured very well.

Here is example webpack config.
Entry is just the first file, which should be executed.
CleanWebpackPlugin cleans build folder after previous builds,
HtmlWebpackPlugin creates index.html with all your scripts and put some HTML tags, you set.
Output part gives folder and filenames for build files.

And here are some loaders.
For example, here we use style and css loaders for .css files,
babel-loader for .js and file-loader for all other stuff.


As you saw, basically webpack config consists of:
loaders, plugins and other minor settings.

Loaders just convert files or chunks.
They're implemented as the only function, which takes file content and returns it as content of CommonJS module.
Here is almost exact implementation of raw-loader. So, it just returns file content as CommonJS module.
module.exports...

So, here some awesome loaders.
babel-loader which run babel,
css and style loaders for convert CSS and adding it to HTML,
file-loader just copies files to build folder and give you public URL,
img-loader minifies images using imagemin,
html-minify-loader minifies html,
and of course you can find more loaders on webpack's site.

Plugins.

They do whatever they want.
They can add hooks to build steps and convert all output.
They can deeply integrate into webpack because
they can register hooks within webpack's build system and access compiler,
and how it works, as well as the compilation.
Therefore, they are more powerful, but also harder to maintain.
Plugins work at bundle or chunk level and usually work at the end of the bundle generation process.
Plugins can also modify how the bundles themselves are created.
Plugins have more powerful control than loaders.

Some plugins.
HtmlWebpackPlugin adds index.html with all scripts and tags, you set.
MiniCssExtractPlugin extracts CSS to external files
SplitChunksPlugin splits your code into smaller parts. We will cover it later. In advance...
DotenvPlugin allows you to save all config into .env file and get it easily in the code.
You can find more plugins at webpack's site.

There is advanced guide.

Code splitting.
Basically, you can just add few entries to webpack config. And webpack will generate 2 files for you:
index and another. One for each entry.

But there is issue with such configuration.
For example, if we had common dependency in those files, then it will be included in both bundles.
So it will be loaded twice. And it's death for performance. It's very bad. So we proceed to next config way.

SplitChunksPlugin.
It is really awesome. It automagically extracts common modules into it's own files. So it prevents such duplication.
It splits all your dependencies and your code into smaller chunks. So here it will extract lodash to it's own file
so it will be loaded only once for 2 files. It's really good way and modern.

Also webpack supports dynamic imports. So we can load our modules... For example, in if clauses and...
So file is loaded when this code is executed, not when the file is executed. There is difference.

Prefetch and preload chunks.
If we put such comment to webpack in the dynamic import, then it will prefetch this module in advance.
Before we even need it. So it will work almost instantly. It's just super cool.

There is some differencies...
There are some differencies between prefetch and preloading.
Preloaded chunk has medium priority and is instantly downloaded.
A prefetched chunk is downloaded only while browser is idle.

Using webpackPreload incorrectly can actually hurt performance, so be careful when using it.

Webpack have development server with live reloading. It's the best solution for live reloading of development website,
when code changes. You can just install it and run webpack dev-server and it will do it for you.
But be careful, this stuff is so addictive.

Caching.
When you deploy site, it’s important to setup caching to save your content on client systems for faster loading next times.
But if you add awesome brand new feature or fix annoying bug you want users to pick up fresh code immediately.
Fortunately, webpack helps us a lot in such mission.

To start we can just add content hash to output filename.
Cause, typical caching algorithm links cached content with file names.
So the easiest way to refresh it — change file name.
We don’t have to do it manually, webpack can help us.

But in real config caching is a bit complicated.
When your bundle consists of few chunks, so make sure to check these docs and articles,
if your project consists of few chunks.
Cause things got complicated then.

Source Map
After build, your code entirely changes.
It’s probably oneliner with even variable names changed. Cat becomes a or n.
You can’t debug it.
There’s were source maps come in. They link generated code with original one.
So you can debug it, see nice stack traces of errors, put breakpoints into dev tools, and see nice code.

To start just add this line to webpack config:
dev-tool: source-map

For production, it's better not to expose original content to end users.
So we need another config for production build.
Generate source maps, but do not link them with original files.
You could just upload them to your error reporting service after.

webpack configuration is very powerful, but quite complicated right now.
Thank you very much for your attention.