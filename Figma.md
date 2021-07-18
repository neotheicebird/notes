## Notes based on udemy course on Figma

A tool to design frontend design on apps and webapps. This is particularly useful when working with a team and it would be advantageous to communicate our vision of a product to other investors, other members of team. With figma we can design, collaborate and prototype.

Check out this article on 10 reasons why Figma is essential
https://ymedialabs.medium.com/10-designers-share-10-reasons-why-figma-is-an-essential-tool-for-design-collaboration-20254e1e83bc#:~:text=As%20a%20motion%20designer%2C%20Figma,of%20what%20I'm%20designing.


Also check out a design company https://yml.co/ who use Figma predominantly.


## building a wireframe app idea
Ref: https://www.youtube.com/watch?v=dXQ7IHkTiMM

Editor: is 65000pts across on all 4 directions

Frame: We can use `F` or click the # symbol on top to add a frame. A frame can be set to a dimension on the right sidebar, say Google Pixel XL. Click on the Frame tool, select the preset dimension and then draw or click on the editor to get the frame

Pan and Zoom - Ctrl + mouse scroll, Ctrl + +/- for zoom, Spacebar + drag for pan

Layers - A frame is a new layer, the layer panel has heirarchy, with a frame being the parent and assets could be children. Say we are designing a insta like post, we can add a rectangle for the content, an avatar circle at the top left, a username etc. We can group them together and convert them into a layer, so that its easier to duplicate and work with. So layers are more than just starting frames, infact in figma, each shape added is called a layer.

Stroke and Fill - We can add a shape using the rectange tool's dropdown. After adding a shape to the editor, on the right sidebar, we can change the fill or stroke value using the Fill and Stroke segments.

Alignment - A layer in a frame can be aligned wrt to the frame, or a shape can be aligned wrt other shapes using the alignment section in the right sidebar.

Group - To create a group out of a set of layers, we can use select a set of layers and do Ctrl + G. In the layers panel, they will now appear under a Group item. In groups we cannot apply properties to layers in bulk, frames on the other hand has this feature. So we can convert a group into a frame by selecting the group in the left sidebar and from the right sidebar, change from Group to Layer.

Duplicate - Click on a layer, press alt and drag or use Ctrl + D

Components and Libraries - Libraries are collection of components, where component are the basic building blocks of our design.

Design System - A combination of component libraries and standards and guidelines for implementing them in code. They are a single source of truth for designers and developers. Helps create consistency in scale.

Community - Figma community is where designers share content, like wireframes and components. We can duplicate it and use it in our projects. By clicking `Duplicate` on any community project, it gets added to our drafts. We can then click on the file name dropdown on the top bar and click on `Publish styles and components` to publish as a library and use components in our project.

Instances - Copies of a main component. Changing the main component, changes all instances. This saves time. For example, a single button type can be instantiated and worked upon. We can change color of all these buttons instantly, keeping specific properties like button text.

Constraints - Ultimately, we design for the web and have to support various screen sizes. Constraints help make the layers responsive. By default a layer is set to left and top constraint, we can change it to center or left&right, so that if we resize the frame, the layers respond to the resize.

Additional screens - Press F and add another google pixel frame. We can shift + select common components on first frame, like status bar and Fab button, copy it and paste it into the new frame.

Additional Pages - In the left sidebar, click on the Page select dropdown, click on Plus icon and create a new page. For example,we can have 2 pages "wireframes" and "designs"

Alt/Option - Click on an object and hover outside with alt pressed to find the distance between the frame edges and the object.

Grid system - To add a grid system, select the frame and in right sidebar, add a new Layout Grid and select grid resolution.

Corner Radius - Click on a rectangle and on rightsidebar, click on corner radius and set the radius.

Place image - Use Ctrl+Shift+K or Place image button in toolbar to select multiple images. These images along with the preview of image to be placed will be shown on cursor, click on objects to fill images.

Text Styles - Create a text and in right sidebar, click on text styles and create a style. Now we can apply this style to texts in various places and modify all the elements at once by editing the style.

Color styles - We can also have styles to apply for colors, strokes etc. To create a color style, click on the grid-like icon on the fill section of right sidebar.

Plugins - Click on a layer, rightclick > Plugins > Select a plugin that is already installed from community, to get extra features. For example, the plugin `stark` can be used to check contrast of text. To use the last plugin use Ctrl + Alt + P

Autolayout - Create designs that grow to fill or shrink to fit as content changes. Auto layout only works in one direction horizontal or vertical. Select a set of layers and use shortcut Shift + A to autolayout them vertically

Components - Select a set of layers (or better the master group layer)  and click on create component from toolbar (Ctrl + Alt + K). It is better to create a page for components and move your newly created component to this page, by rightclicking on the component and click move to page > page name (Components).

Scrolling - Click on a frame and select prototype > behaviour > vertical scrolling. You can click on any layer in the frame and change overflow behaviour for that item to None to keep it in position while scrolling, like for NavBar.


