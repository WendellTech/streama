Thanks for wanting to contribute with a translation for the app! 

Here are the steps to add a new language to Streama: 

### 1. Fork
First of all you'll need to make a fork and open it up in something like Intellj IDEA.

### 2. Copying the template
For easier translation, you should make a copy of the file `grails-app/assets/javascripts/streama/translations/EN_us.js` In the same directory and call it by your languages code. See [this](http://www.lingoes.net/en/translator/langcode.htm) for reference (even-though in streama the capitalization is reversed :P). 

### 3. Translate
Make all necessary translations there.  
Also, In line 5 of that file make sure to call the translation by the language code (or the abbreviated language code, that is up to you).  

### 4. Add your language to the app
So now you just need to tell the app about your language. First, go through each of the other translation files and add your language in the same format as the others, for example: 

```javascript
LANGUAGE_de: 'German',
LANGUAGE_fr: 'French',
...
```

Then head on over to `/grails-app/assets/javascripts/streama/streama.translations.js` and add the string you used in line 5 of your translation-file to the available languages array. 
```javascript
 $rootScope.availableLanguages = ['en', 'fr', ...];
```

### 5. Test and submit PR 
In the user profile it should now show up as selectable from a dropdown list. Choose it and test!  

*Note*  
If you find any parts that still need translating let me know. I chose to limit translations only to the Frontend of the application for now, the admin area is English. 
