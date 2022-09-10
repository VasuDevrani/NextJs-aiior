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
    meetUps: {
     meets: data
    }
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

### Exploring static generation
- npm run build
- got to the 'next' folder created as a result of build command
- checkout 'server' folder and you will get the data pre-rendered in the html files and json files that contains the pre rendered fetched data.

### Exploitation of data during website rendering
- If the site route is approached directly, then the server serves pre-rendered html page
- if the site route is approched from another route, then json & html files are served

## getStaticPaths
```
export async function getStaticPaths() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts");
  const data = await res.json();

  const paths = data.map((item) => {
    return {
      params: {
        postId: `${item.id}`,
      },
    };
  });

  return {
    paths,
    fallback: false,
  };
}
```
- defines the paths for dynamic SSGs.
- contains a fallback param, which can be true, false, or blocking
- If fallback is true => will create new req for new paths, and add new statically generated page, data to the static part.
  - a if counter statement is needed
  - ```
    if(router.fallback)
    {
      return (
        <h1> loading... </h1>
      )
    }
  - for the very first req, a fallback html page (here its loading) is returned and a newly pre-rendered html page with JSON data is being pre-rendered in the server.
  - for any subsequent req, this page will act like other pages, that is pre-rendered from the very start, no new req
- If fallback is false => any other path other than those mentioned in the getStaticPaths will not be rendered and a 404 page would be served
- If fallback is 'blocking' => the page will load slow and the process would be same as fallback true, no need of if on loading is slow

### INCREMENTAL STATIC GENERATION


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
