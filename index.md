You have successfully installed MODX Revolution \[\[++settings\_version\]\]!

Now that MODX is installed you can login to the manager to create your templates, manage content and install third party extras to add functionality to your website.

<iframe frameborder="0" height="300" src="http://jsfiddle.net/joshnh/srNVQ/embedded/" width="100%"></iframe>

New to MODX?
------------

Pages on a MODX site are called [Resources](https://rtfm.modx.com/revolution/2.x/making-sites-with-modx/structuring-your-site/resources), and are visible on the left-hand side of the manager in the Resources tab. Resources can be nested under other resources, making it easy to create a tree of resources. There are different types of resources for different use cases.

Building your website is done through a combination of **Templates**, **Template Variables**, **Chunks**, **Snippets** and **Plugins**. Collectively these are known as **Elements**, and can also be found in the left-hand side of the manager, in the Elements tab.

[Templates](https://rtfm.modx.com/revolution/2.x/making-sites-with-modx/structuring-your-site/templates) contain the outer markup of any page. Each resource can only be assigned to a single template at a time. By adding [Template Variables](https://rtfm.modx.com/revolution/2.x/making-sites-with-modx/customizing-content/template-variables) to a template, you can add custom fields for any resource using that particular template.

With [Chunks](https://rtfm.modx.com/revolution/2.x/making-sites-with-modx/structuring-your-site/chunks) you can share parts of the markup, such as a header, across different templates. [Snippets](https://rtfm.modx.com/revolution/2.x/making-sites-with-modx/structuring-your-site/using-snippets) are pieces of PHP that return dynamic content, such as summaries of other resources or the current date. With snippets, you will often use Chunks to mark up the pieces of content it returns, instead of mixing the PHP and HTML.

Finally, [Plugins](https://rtfm.modx.com/revolution/2.x/developing-in-modx/basic-development/plugins) enable more advanced features by hooking into the extensive events system provided by MODX.

To learn more about MODX, be sure to check out the [Getting Started](https://rtfm.modx.com/revolution/2.x/getting-started) section in the official documentation.