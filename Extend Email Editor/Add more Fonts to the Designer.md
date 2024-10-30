
# Add more Fonts to the Designer

> [!IMPORTANT]
> **Attention**: Since summer 2024 this feature is an out-of-the-box-feature in customer insights.

Out of the box, Microsoft Dynamics 365 Marketing's email and page designers come pre-equipped with a selection of web-safe fonts. These web-safe fonts are chosen for their compatibility across a wide range of devices, ensuring that the design elements of your marketing emails and pages maintain a consistent appearance for recipients.
Web-safe fonts are fonts that are widely supported by various operating systems and email clients, reducing the risk of your content appearing incorrectly or being unreadable due to font compatibility issues. These fonts have been carefully selected to strike a balance between visual aesthetics and cross-platform compatibility.
While it's true that some marketers might find these web-safe fonts somewhat limited in terms of creative expression, the primary objective is to ensure that your marketing materials reach your audience as intended. By using web-safe fonts, you can minimize the chances of text being replaced with default fonts or appearing distorted on certain devices or email clients.
However, it's worth noting that Microsoft Dynamics 365 Marketing also provides options for more advanced users to customize font choices. If a marketer desires a specific font that is not included in the default web-safe selection, they can explore opportunities to incorporate custom fonts. This might involve working with cascading style sheets (CSS) or utilizing other techniques to import and utilize non-web-safe fonts within the platform's design capabilities.
In summary, while the initial selection of web-safe fonts in Microsoft Dynamics 365 Marketing's email and page designers might not cater to every marketer's design preferences, they serve a crucial role in ensuring consistent and reliable rendering of content across various devices and platforms. For those seeking greater creative freedom, the platform does offer avenues for exploring additional font options to align more closely with their branding and design objectives.

![Fonts](/img/Extend%20Email%20Editor/font.png)
Here's a detailed explanation of how to add custom web fonts and ensure they appear in the dropdown menu, allowing your users to work with them seamlessly.

> [!NOTE]
>  Perform these steps within the Email Template or Marketing Page Template so that when your users use one of those templates to begin designing a new email or page, they’ll automatically have access to the new web fonts.

## Step 1: Selecting Custom Web Fonts
- Before you begin, make sure you have the desired custom web fonts already available and accessible via the internet. These can be free or licensed fonts found on various font websites. My recommendation is [Google Fonts ](https://fonts.google.com/)
- Select a Font, click plus and copy the “Use on the Web” Code

![Fonts](/img/Extend%20Email%20Editor/fonts.png)

The Code looks like this:
```
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Open+Sans&display=swap" rel="stylesheet">
```

## Step 2: Accessing the Email or Page Design Feature
 - Log in to your Microsoft Dynamics 365 Marketing account and navigate to the area where you can create or edit Email Templates or Marketing Page Templates.

## Step 3: Adding the Web Fonts
- Open the desired Email or Marketing Page Template in which you wish to use the custom web fonts.
- Open the HTML Designer
- Paste the Code in the HTML of the E-Mail

![Fonts](/img/Extend%20Email%20Editor/fonteditor.png)

## Step 4: Incorporating Custom Web Fonts

- Paste this Code in your HTML Editor:
  `<meta type="xrm/designer/setting" name="additional-fonts" datatype="font" value="<font-list>">`
- In the value attribute, we just need to list our custom font or fonts, separating them by a semi-colon:
  Like in here: 
  `<meta type="xrm/designer/setting" name="additional-fonts" datatype="font" value="Open Sans">`
  
![Fonts](/img/Extend%20Email%20Editor/fonteditornew.png)

## Step 5: Save and Use

- After adding the custom web fonts, don't forget to save your template.
- Now, the custom web fonts should be available in the font dropdown menu when your users use this template to design a new email or page.

![Fonts](/img/Extend%20Email%20Editor/fontsdone.png)


By following these steps within the Email Template or Marketing Page Template, you can ensure that the custom web fonts you've chosen are easily accessible and can be used by your users to create creative projects.
