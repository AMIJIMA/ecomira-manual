【障害対応記録】

発生日：
2026年7月9日

件名：
keihou@ecomira.co.jp からの警報・確認メールが届かなかった件

影響範囲：
以下の宛先すべてにメール未着
・keihou@ecomira.co.jp
・amijimah@ecomira.co.jp
・mny.miyata@softbank.ne.jp
・mny.n.kuramata@gmail.com

通常動作：
毎朝6:00にVPS上のcronから以下のスクリプトを実行
/root/ecomira/vps_power_check_list.py

確認内容：
・VPS管理画面でサーバーが「停止中」であることを確認
・VPSを起動
・SSHログイン後、uptime により起動直後であることを確認
・cron は active running を確認
・crontab -l にて、6:00実行の設定を確認
・過去ログでは 2026/07/07、2026/07/08 は success=True を確認
・2026/07/09 6:00 のログはなし
・2026/07/09 11:43 に手動実行し、success=True を確認

原因：
VPSが朝6:00時点で停止していたため、cronが実行されず、警報メール送信処理が動かなかった。

復旧対応：
・VPSを起動
・VPS契約を自動更新に設定
・手動で以下を実行
/usr/bin/python3 /root/ecomira/vps_power_check_list.py
・ログで success=True を確認

再発防止：
・VPS契約を自動更新に設定済み
・VPS停止時に別メールへ通知が届く仕組みを検討
・keihouメールが届かない場合の確認手順を手順書化する

対応者：
網島弘幸

備考：
警報メールの送信元サーバー自体が停止すると、異常通知も届かない。
そのため、VPS会社側の契約期限通知・停止通知は、VPSに依存しないメールアドレスにも送る必要がある。