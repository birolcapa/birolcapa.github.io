---
layout: post
title:  "Open XML: Removing Paragraphs From Template"
date:   2020-04-16 22:23:40 +0200
categories: software
---
I needed to generate a word document.

First requirement was: I must use a **template**.  
Second requirement was: The **template** is given by third party and cannot be changed.  
Last requirement was: Generated document shall not have some parts of the **template**.  

To do so, I used Open XML SDK and C#.

So I was lucky that someone already had a similar problem: <https://stackoverflow.com/q/22234453/1458850>  

And the answer worked for me:  
<https://stackoverflow.com/a/22235277/1458850>

I use the knowledge above and created my solution as following:

```csharp
public void RemoveParagraphsFromDocument(string begin, string end)
{
    using (var wordDoc = WordprocessingDocument.Open(OutputPath, true))
    {
        var mainPart = wordDoc.MainDocumentPart;
        var doc = mainPart.Document;
        var paragraphs = doc.Descendants<Paragraph>().ToList();
        var beginIndex = paragraphs.FindIndex(par => par.InnerText.Equals(begin));
        var endIndex = paragraphs.FindIndex(par => par.InnerText.Equals(end));

        for (var i = beginIndex + 1; i < endIndex; i++)
        {
            paragraphs[i].Remove();
        }

        doc.Save();
    }
}
```