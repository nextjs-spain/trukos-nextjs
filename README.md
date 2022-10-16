# Trukos Nextjs

# Comandos
```
npx create-next-app partcombinator-nextjs  --use-npm
npx create-next-app .
npm run dev
```

# Boostrap
```
import 'bootswatch/dist/cosmo/bootstrap.min.css'
import '../global.css'

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
```

# Layout

```
import React from 'react'
import Navbar from './Navbar'
import { useEffect } from 'react'
import { useRouter } from 'next/router';
import nProgress from 'nprogress';

export default function Layout({children, footer = true, dark = false}) {
  const router = useRouter();

  useEffect(() => {
      const handleRouteChange = url => { 
          console.log(url);
          nProgress.start();
        }

      router.events.on('routeChangeStart', handleRouteChange);
      router.events.on('routeChangeComplete', () => nProgress.done());

      return () => {
        router.events.off('routeChangeStart', handleRouteChange);
      }
  }, [])

  return (
    <div className={ dark ? 'bg-dark' : '' }>
        <Navbar />
        <main className='container py-4'>
          {children}
        </main>

        {
          footer && (
            <footer className="bg-dark text-light text-center">
              <div className="container p-4">
                  <h1>&copy; My Portfolio</h1>
                  <p>2000 - {new Date().getFullYear()}</p>
                  <p>All right Reserved</p>
              </div>  
            </footer>
          )
        } 
    </div>
  )
}
```

# Uso de Link
```
import Layout from "../components/Layout";
import Link from "next/link";

const Custom404 = () => (
  <Layout>
    <div className="text-center">
      <h1>404</h1>
      <p>This page does nor exists. Please return to 
        <Link href="/"><a>Home</a></Link></p>
    </div>
  </Layout>  
)


export default Custom404;
```

# Llamada a API desde el servidor.

```
import Layout from "../components/Layout";
import Error from "./_error";

const Github = ({user, statusCode}) => {


  console.log(user)
  
  if (statusCode) {
    return <Error statusCode={statusCode} />
  }

  
  return (
      <Layout footer={false}>
          <div className="row">
              <div className="col-md-4 offset-md-4">
                  <div className="card card-body text-center">
                      <h1>{user.name}</h1>
                      <img src={user.avatar_url} alt="user" />
                      <p className="mt-2">{user.bio}</p>
                      <a href={user.blog}  target="_blank" 
                          className="btn btn-secondary m-2">My Blog</a>
                      <a href={user.html_url}  target="_blank" 
                         className="btn btn-secondary m-2">Github</a>
                  </div>
              </div>
          </div>
      </Layout>  
    )
}

export async function getServerSideProps(){
  const res = await fetch('https://api.github.com/users/falconsoft3d');
  const data = await res.json();

  const statusCode = res.status > 200 ? res.status : false;

  console.log(data);
  return {
    props: {
      user: data, 
      statusCode
    }
  }
}


export default Github;
```
