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
- in normal react data is usually fetched using a useEffect i.e. in the first cycle all variables are assigned memory then data is fetched and stored.
- this can be improved by generating this data as pre-rendered
- NextJS does pre rendering using build pre rendering - getStaticProps() or server-side rendering
- this is seo friendly as the fetched data is initially present in the html page

## getStaticProps()
```
export async function getStaticProps() { 
  // pre-renders the data during the build process
  // for dynamic changes in fetched data use revalidate
  
  props: {
    meetUps: .....
    revalidate: 1 //number of seconds for refetch cycle
  }
}

//using for fetching data acc to params.id
//we can't access the params id using the useRouter hook as it works only inside the first level of component function

export async function getStaticProps(context) { 
  const id = context.params.id;
  
  props: {
    meetUps: .....
    revalidate: 1 //number of seconds for refetch cycle
  }
}
```

## Server side rendering using getSerevrProps()
```
export async function getServerProps(context){
   const req = context.req;
   const res = contxt.res;
   
  props: {
    
  }
}
```
