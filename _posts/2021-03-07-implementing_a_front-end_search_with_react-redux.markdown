---
layout: post
title:      "Implementing a front-end search with React-Redux"
date:       2021-03-07 19:11:52 +0000
permalink:  implementing_a_front-end_search_with_react-redux
---


![demo](https://i.imgur.com/4zv0DXE.gif)

This is a quick tutorial showing how to search your Redux store using Fuse for configurable fuzzy searching resulting in a React and Bootstrap modal showing the results.

Initially I set up a search that queried the backend - in my case a Rails API but after some thinking I decided that because I already stored all the backend data in my Redux store that it would be better and quicker to just search through that instead.

So in the demo you can see that I have a search-bar set up in my navbar. This is a Formik form - take a look at the onSubmit function.

```
<Formik
          initialValues={{ keyword: ""}}
          onSubmit={(values, { setSubmitting, resetForm }) => {
            setSubmitting(true);
            this.setState({keyword: values })
            const results = this.fuseSearch(this.state.keyword.keyword)
            this.setState({results})
            setSubmitting(false);
            this.setState({modalShow: true}) 
            resetForm();
          }}
        >
          {({
            touched,
            errors,
            handleSubmit,
            isSubmitting,
          }) => (
            <Form className="row pr-20 vertical-center" onSubmit={handleSubmit}>
              <Form.Group className="col-9 pr-0 col-md-8">
                <Field
                  type="search"
                  placeholder="Search for hikes"
                  className={'form-control ' + (errors.keyword && touched.keyword ? 'is-invalid' : '')}
                  size="sm"
                  name="keyword"
                />
              </Form.Group>
              <Form.Group className="col-1 pl-0 pr-0">
              <Button variant="outline-success" className="d-flex" type="submit" disabled={isSubmitting}>
                Search
              </Button>
              </Form.Group>
            </Form>
          )}
        </Formik>
```

if you're not familiar with Formik it may be a bit confusing but basically its disabling the form from being submitted again, setting the local state with the keyword that has been entered. Then...

```
const results = this.fuseSearch(this.state.keyword.keyword)
```
This line is using the [Fuse](https://fusejs.io/getting-started/installation.html) module and passing it the keyword.

Here is the Fuse function configuration.

```
fuseSearch = (keyword) =>  {
    const hikes = this.props.hikes
    const fuse = new Fuse(hikes, {
      threshold: 0.3,
      keys: [
        'title',
        'difficulty',
        'location',
        "description",
      ]
    });
    const matches = fuse.search(keyword)
    return matches.map(({item}) => {
      return item
    });
  }
```
This function is fairly easy to set-up, the constant 'fuse' is just inputing what things you want to search through and the 'matches' are then returned.

The results are then passed into my modal component like so

```
<ModalHikes
          show={this.state.modalShow}
          onHide={() => this.setState({modalShow: false})}
          keyword={this.state.keyword}
          hikes={this.state.results}
        />
```

The 'ModalHikes' component then renders the data...

```
export default function ModalHikes(props) {
  let hikes = props.hikes || []
    const hikeList = hikes.map((hike) => {
        return <Hike key={hike.id} hike={hike} picture={"150"}/>;
    })

    return (
        <Modal
          {...props}
          size="md"
          centered
        >
          <Modal.Body>
            <h3>Results for '{props.keyword.keyword}'</h3>
            {hikeList}
            {hikes.length === 0 ? "Sorry no hikes found" : null}
          </Modal.Body>
          <Modal.Footer>
            <Button onClick={props.onHide}>Close</Button>
          </Modal.Footer>
        </Modal>
      );
}

```

There you have it, a functioning (only just) fuzzy search of the Redux store which outputs the result in a nice little modal.

