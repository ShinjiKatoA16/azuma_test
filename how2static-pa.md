# How to set up static HP on pythonanywhere

- Create django application and deploy to pythonanywhere
- Login to pythonanywhere
- Click **Web** tab
- Scroll down to **Static Files:**
- Create new entry  url を / に、Directory を自分のホーム上の html を置くところ（例： /home/*あなたのUserID*/static-html)
- Login to github
- 新しい Repository を作成、名前は pythonanywhere で指定した Directory ば便利 (例： static-html)
- Local PC から github に data をコピー

1. cmd を開く
2. cd **html がはいっている Directory**
3. `git init`
4. `git add --all`
5. `git commit -m "initital"`
6. `git remote add origin https://github.com/usre-abc/static-html.git`
7. `git push -u origin master`
   
- pythonanywhere で bash terminal を開いてhtml をコピー

1. `git clone httmls://github.com/user-abc/static-html.git`
   
- pythonanywhere の **web** tab で **Reload** をクリック


# 一度作った後の変更方法

- Local PC でファイルを変更
1. `git add --all`
2. `git commit -m "comment"`
3. -git push`

- pythonanywhere で変更をひろう
1. `git pull`
