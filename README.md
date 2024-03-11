To add a typo checker in project by using eslint-plugin needs to follow below steps:
## Step 1 : install and set config of eslint
```bash
npm init @eslint/config
```

It asks some details:
> How would you like to use ESLint? 
* To check syntax only 
* To check syntax and find problems 
* To check syntax, find problems, and enforce code style 

choose as you need. I choose option 2 "To check syntax and find problems"

> What type of modules does your project use? … 
* JavaScript modules (import/export) ,
* CommonJS (require/exports) ,
* None of these

choose as you need. I choose option 1 "JavaScript modules (import/export)"

> Which framework does your project use? … 
* React ,
* Vue.js ,
* None of these

choose as you need. I choose option 1 "React"

> Does your project use TypeScript? ‣ No / Yes

If you are using `typescript` then choose `yes` otherwise `No` .
I choose yes

> Where does your code run? 
* ✔ Browser ,
* ✔ Node

Choose Browser

> What format do you want your config file to be in? … 
* JavaScript
* YAML
* JSON

choose format of `.eslintrc` file type 
I choose `JSON`

Now install dependencies which suggested by eslint
If it shows error while installing dependencies then you can manually install dependencies using --force

In above case I use
```bash
 npm i --force @typescript-eslint/eslint-plugin@latest eslint plugin-react@latest @typescript-eslint/parser@latest eslint@latest
```

Now, a eslintrc.json file is created in your root directory

## Step 2: Install spell-check plugin
```bash
    npm install eslint-plugin-spellcheck
```

put below code in `eslintrc.json` file in rule section
````
"spellcheck/spell-checker": ["error",
       {
           "comments": true,
           "strings": true,
           "identifiers": true,
           "templates": true,
           "lang": "en_US",
           "skipWords": [
               "dict",
               "aff",
               "hunspellchecker",
               "hunspell",
               "utils"
           ],
           "skipIfMatch": [
               "http://[^s]*",
               "^[-\\w]+\/[-\\w\\.]+$"
           ],
           "skipWordIfMatch": [
               "^foobar.*$"
           ],
           "minLength": 3
        }
    ]
````
    
and add `spellcheck` in plugins array For eg :-  
````
"plugins": ["@typescript-eslint", "react", "spellcheck"]
````

and add 
````
"settings": {
    "react": {
      "version": "detect"
    }
  }
````
 to detect version of react automatically


This is used to set the rules of spell checking
Ref : https://www.npmjs.com/package/eslint-plugin-spellcheck

## Step 3 : Add a script in `package.json` file in script section
`"lint": "eslint ."`

This is done to run the linter manually

Now, eslint and spell check is installed in your project .
You can run 
```bash
npm run lint
```

It should detect spelling error in your code.

## Step 4: Now , if you want to run eslint just before commiting to github for this it needs to install `husky`. 
```bash
npm i husky
```

and add a script in `package.json` file in script section :- `'prepare' : "instal husky"` . This is done to ensure that every collabarator has install husky
```bash
npx husky-init
```
to make config of husky in project

Now, a `.husky` folder is created in root directory .
Go to `.husk` folder and open `pre-commit` file and replace `npm test` to `npm run lint` to run you linter before commiting you file automatically.
Now you can check if lint is running when commit happens 

## Step 5: To run only those files which have changes . Don't want to run linter for whole codebase when commiting for this it needs to install lint-staged package 
```bash
npm install --save-dev lint-staged
```
Now, Inside `.husky/pre-commit` replace `npm run lint` with `npx lint-staged`

Now , in your root directory create a file `.lintstagedrc.json` and add 
````
{
  "*.{js,jsx,ts,tsx}": "eslint --ignore-path .gitignore --cache --fix",
  "*.html": ["eslint --ignore-path .gitignore"]
}
````
in this file.

Now, lint-staged is setup in project.

You can test it by commiting in you project



