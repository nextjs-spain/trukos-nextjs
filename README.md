# trukos-nextjs


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
