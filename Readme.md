## Tracking whatever I learn in NextJS

Features that NextJS provides:
- File routing
- The index.js file represents the '/' endpoint
- the names of folder or files in pages determine the endpoints
- NextJS provides the server side rendering helps in initiation and works as React i.e. provides client side rendering later
- useRouter() hook used to access params id
  - ```
    const router = useRouter()
    const id = router.query.id 
    or, 
    router.push('/') works as useNavigate
    ```
- Link tag works with href comes under 'next/link'

## Working with fetching data
- all below stuff can be done using react practices as well
- in normal react data is usually fetched using a useEffect i.e. in the first cycle all variables are assigned memory then data is fetched and stored.
- this can be improved by generating this data as pre-rendered
- NextJS does pre rendering using build pre rendering - getStaticProps() or server-side rendering
- this is seo friendly as the fetched data is initially present in the html page

## RENDERING

Next.js has two forms of pre-rendering: Static Generation and Server-side Rendering. The difference is in when it generates the HTML for a page.

- Static Generation (Recommended): The HTML is generated at build time and will be reused on each request.
- Server-side Rendering: The HTML is generated on each request.

### getStaticProps()
```
export async function getStaticProps() { 
  // pre-renders the data during the build process
  // for dynamic changes in fetched data use revalidate
  
  props: {
    meetUps: .....
    revalidate: 1 //number of seconds for refetch cycle
  }
}
```

### using for fetching data acc to params.id

we can't access the params id using the useRouter hook as it works only inside the first level of component function
```
export async function getStaticPath(){
  fallback: false,
  paths: [
    //all params as object
  ]
}

export async function getStaticProps(context) { 
  const id = context.params.id;
  
  props: {
    meetUps: .....
    revalidate: 1 //number of seconds for refetch cycle
  }
}
```

### Server side rendering using getServerProps()
These are rendered in server side only so can be used to hide credentials as well. </br>
Renders only once

```
export async function getServerProps(context){
   const req = context.req;
   const res = contxt.res;
   
  props: {
    // passed to the page as props
  }
  // no revalidation
}
```

## BACKEND Integration
NextJs acts as a full stack framework, allowing us to create backend api routes within the same frontend code base </br>
- Just create a 'api' folder inside the pages
- each file/folder name will act as api route endpoints as <b>/api/endpoint</b>
- These are rendered within server only and never passed to client machine

## Head MetaDATA for SEO
- using the Head component present in 'next/head'
```
<Head>
  <title>Delhi MeetUps</title>
  <meta name='description' content='This site is the best site around the globe'/>
</Head>
```
- we can do this is every single page by wrapping the code inside fragmentsx

## DEPLOYMENT
Deploy using Vercel, the creators of Next.js
