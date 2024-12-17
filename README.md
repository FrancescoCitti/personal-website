
# Francesco Citti - Personal Website 🚀

Welcome to the source code for my personal portfolio and blog, built using **Hugo** with the **hello-friend-ng** theme. This website showcases my projects, CV, and cybersecurity work.

---

## 🛠️ **Tech Stack**

- **Framework**: [Hugo](https://gohugo.io/) (Static Site Generator)
- **Theme**: [hello-friend-ng](https://themes.gohugo.io/themes/hello-friend-ng/)
- **Hosting**: [GitHub Pages](https://pages.github.com/)
- **Styling**: SCSS and Monokai for code fences
- **Automation**: GitHub Actions to deploy changes to the `gh-pages` branch

---

## 📄 **Website Content**

The website includes the following sections:

1. **Home**: An introduction and personal tagline.
2. **About**: A casual, personal overview of who I am and what I do.
3. **CV**: My curriculum vitae showcasing work experience, education, and certifications.
4. **Projects**: Detailed writeups of cybersecurity and DevSecOps projects.
5. **Blog**: Posts and articles on cybersecurity, cloud automation, and tech walkthroughs.

---

## ⚙️ **Setup and Deployment**

### **1. Installation**
To clone and run this project locally:

```bash
# Clone the repository
git clone https://github.com/francescocitti/personal-website.git  
cd personal-website  

# Install Hugo (ensure you have the extended version)
brew install hugo  

# Run the local server
hugo server
```

The website will be available at [http://localhost:1313](http://localhost:1313).

---

### **2. Deployment**

This website uses **GitHub Actions** to deploy updates automatically:
- The `main` branch contains the source code.
- A GitHub Action builds the site with Hugo and deploys the `public/` folder to the `gh-pages` branch.

To trigger a deployment, simply commit and push changes to the `main` branch:

```bash
git add .  
git commit -m "Update content"  
git push origin main  
```

---

## 🖼️ **Customization**

### **1. Portrait Image**
Place your profile picture here:
```
static/img/image.jpg
```

Update the `hugo.toml` file:
```toml
[params.portrait]
  path = "/img/image.jpg"
  alt  = "Francesco Citti"
```

### **2. Social Links**
Customize your social icons in `hugo.toml`:
```toml
[[params.social]]
  name = "github"
  url  = "https://github.com/francescocitti"

[[params.social]]
  name = "linkedin"
  url  = "https://www.linkedin.com/in/yourprofile"
```

---

## 🔗 **Live Website**

Visit the live site here: [https://francescocitti.com](https://francescocitti.com)

---

## 🧰 **Future Improvements**

- Add automated comments using **Utteranc.es** or **Commento**.
- Integrate **Plausible Analytics** for privacy-friendly traffic monitoring.
- Implement a dark/light theme toggle.

---

## 📜 **License**

This project is licensed under the [Creative Commons Attribution-NonCommercial 4.0 International License](https://creativecommons.org/licenses/by-nc/4.0/).

---

## 👨‍💻 **Author**

**Francesco Citti**
- GitHub: [@FrancescoCitti](https://github.com/FrancescoCitti)
- Email: [francesco.citti@tuta.com](mailto:francesco.citti@tuta.com)  
