# Novo diretório com estrutura compatível com Vercel
vercel_project_dir = "/mnt/data/perfil-api-vercel"
api_dir = os.path.join(vercel_project_dir, "api")
os.makedirs(api_dir, exist_ok=True)

# Conteúdo do perfil.js (dentro de /api/)
perfil_js_content = '''\
export default function handler(req, res) {
  const {
    idseila = "nunes",
    coins = "1.7B",
    reps = "169",
    status = "Mudei de con",
    aboutMe = "patinn",
    banner = ""
  } = req.query;

  res.status(200).json({
    username: idseila,
    level: 21,
    coins: coins,
    reps: reps,
    status: status,
    aboutMe: aboutMe,
    banner: banner || "https://via.placeholder.com/800x300?text=Banner+Default",
    avatar: "https://via.placeholder.com/128?text=Avatar"
  });
}
'''

# Conteúdo do package.json (opcional, Vercel pode rodar sem isso)
package_json_content_vercel = '''\
{
  "name": "perfil-api-vercel",
  "version": "1.0.0"
}
'''

# Salvar arquivos
with open(f"{api_dir}/perfil.js", "w") as f:
    f.write(perfil_js_content)

with open(f"{vercel_project_dir}/package.json", "w") as f:
    f.write(package_json_content_vercel)

# Compactar em .zip
vercel_zip_path = "/mnt/data/perfil-api-vercel.zip"
with zipfile.ZipFile(vercel_zip_path, 'w') as zipf:
    for root, dirs, files in os.walk(vercel_project_dir):
        for file in files:
            file_path = os.path.join(root, file)
            arcname = os.path.relpath(file_path, vercel_project_dir)
            zipf.write(file_path, arcname)

vercel_zip_path
