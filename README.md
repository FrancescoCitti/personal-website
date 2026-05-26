
# Francesco Citti — Personal Website

Personal portfolio built with **Hugo** and the **hello-friend-ng** theme. Covers projects, CV, and photography.

---

## Tech Stack

* **Framework**: Hugo (Static Site Generator)
* **Theme**: hello-friend-ng
* **Hosting**: GitHub Pages
* **Styling**: SCSS and Monokai for code blocks
* **Automation**: GitHub Actions deploys to `gh-pages` on push

---

## Content

1. **Home**: Introduction and tagline.
2. **About**: Personal overview.
3. **CV**: Work experience, education, and certifications.
4. **Projects**: Cybersecurity and DevSecOps writeups.
5. **Photography**: Photo albums.

---

## Setup and Deployment

### Installation

```bash
git clone https://github.com/francescocitti/personal-website.git
cd personal-website

# Requires the extended version of Hugo
brew install hugo

hugo server
```

Available at [http://localhost:1313](http://localhost:1313).

---

### Deployment

Push to `main` — GitHub Actions builds and deploys to `gh-pages` automatically:

```bash
git add .
git commit -m "Update content"
git push origin main
```

---

## Customization

### Portrait Image

Place your profile picture at `static/img/image.jpg` and update `hugo.toml`:

```toml
[params.portrait]
  path = "/img/image.jpg"
  alt  = "Francesco Citti"
```

### Social Links

```toml
[[params.social]]
  name = "github"
  url  = "https://github.com/francescocitti"

[[params.social]]
  name = "linkedin"
  url  = "https://www.linkedin.com/in/yourprofile"
```

---

## Live Site

[https://francescocitti.com](https://francescocitti.com)

---

## License

[Creative Commons Attribution-NonCommercial 4.0](https://creativecommons.org/licenses/by-nc/4.0/)

---

## Author

**Francesco Citti**
* GitHub: [@FrancescoCitti](https://github.com/FrancescoCitti)
* Email: [myservices@francescocitti.com](mailto:myservices@francescocitti.com)
