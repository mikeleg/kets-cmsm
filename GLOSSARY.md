# CMS

The CMS is one content management system for all the showcase websites that mgsoluzioni builds. A Client edits the content of a Site without help from a developer.

## Language

**Site**:
A showcase website that mgsoluzioni builds for one **Client**. It shows the business and has no e-commerce.
_Avoid_: Project, web project, website

**Client**:
The business that owns a **Site** and edits its content. The Client has no technical skills.
_Avoid_: Customer, user, editor

**Installation**:
The running CMS of one **Site**, with its own data. Each Site has exactly one Installation.
_Avoid_: Instance, deployment, tenant

**Account**:
A person who can sign in to the admin of one **Installation**. All Accounts have the same rights. An Account can belong to the **Client** or to the developer.
_Avoid_: User, login, admin

**Page**:
A page of a **Site**. A Page is a list of **Blocks**. A Page can have child Pages, so the Pages of a Site make a tree.
_Avoid_: Screen, view

**Home**:
The one **Page** of a **Site** at the root URL. The **Client** cannot delete or move it.
_Avoid_: Homepage, index, landing

**Draft**:
The state of a **Page** that visitors cannot see. Each new Page starts as a Draft, and the **Client** can put a **Published** Page back to Draft. Edits to a Published Page have no Draft.
_Avoid_: Hidden, unpublished, offline

**Published**:
The state of a **Page** that visitors can see. Each save of a Published Page goes live at once.
_Avoid_: Live, visible, online

**Preview**:
The view of a **Page** on the **Site** frontend that only the **Client** sees, with the edits not saved yet. A Preview never changes what visitors see.
_Avoid_: Draft view, staging

**Menu**:
A list of links of a **Site** that the **Client** orders. Each link goes to a **Page** or to an external URL. Each Site has two Menus: main and footer.
_Avoid_: Navigation, nav, navbar

**Site settings**:
The content of a **Site** that is not in a **Page**, for example name, logo, contacts, social links and default SEO values.
_Avoid_: Global content, configuration, options

**Block**:
A section of a **Page** with a fixed set of **Fields**, for example a hero or a gallery. Each Block has a **Block type**. The **Client** only adds Blocks to a Page and fills their Fields.
_Avoid_: Component, widget, module, section

**Block type**:
The definition of a kind of **Block**: its name and its **Fields**. The base set of Block types is in the CMS. A developer adds a Block type for one **Site** in a file of its **Installation**.
_Avoid_: Block definition, template, schema

**Field**:
One value that the **Client** fills in a **Block**, for example a text, an image or a link. Each Field has a kind from a fixed list and can be required.
_Avoid_: Property, attribute, input

**Language**:
A language in which a **Site** shows its content. Each Site has a list of Languages and one default Language. In the MVP the only Language is Italian.
_Avoid_: Locale, culture, translation

**Contact request**:
A message that a visitor sends with the contact form of a **Site**. The **Client** reads it in the admin and gets it by email.
_Avoid_: Submission, lead, enquiry, message
