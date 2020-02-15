<p>
  <a href="https://melpa.org/#/slack"><img alt="MELPA" src="https://melpa.org/packages/slack-badge.svg"/></a>
  <a href="https://travis-ci.com/yuya373/emacs-slack"><img src="https://travis-ci.com/yuya373/emacs-slack.svg?branch=master" alt="Build Status"></a>
  <a href="https://patreon.com/yuya373"><img src="https://img.shields.io/badge/patreon-Become a patron-052D49.svg?logo=patreon&labelColor=E85B46&logoColor=white" alt="Become a patron" /></a>
</p>
<p align="center"><img src="https://raw.githubusercontent.com/yuya373/emacs-slack/assets/assets/slack-logo.svg?sanitize=true" width=300 height=126/></p>
<p align="center"><b>Emacs Slack</b></p>
<p align="center">GNU Emacs client for <a href="https://slack.com/">Slack</a>.</p>

---

## Preview

You can see some gifs on the [wiki](https://github.com/yuya373/emacs-slack/wiki/ScreenShots).

## Dependencies

- [Alert](https://github.com/jwiegley/alert)
- [circe](https://github.com/jorgenschaefer/circe) (for the Linewise User
  Interface library).
- [Emojify](https://github.com/iqbalansari/emacs-emojify) (optional)
- [Oauth2](https://github.com/emacsmirror/oauth2/blob/master/oauth2.el)
  - do `package install`
- [request](https://github.com/tkf/emacs-request)
- [websocket](https://github.com/ahyatt/emacs-websocket)

## Extensions

- [helm-slack](https://github.com/yuya373/helm-slack)

## Configuration

[How to get token](#how-to-get-token)

```elisp
;; I'm using use-package and el-get and evil

(el-get-bundle slack)
(el-get-bundle yuya373/helm-slack) ;; optional
(use-package helm-slack :after (slack)) ;; optional
(use-package slack
  :commands (slack-start)
  :init
  (setq slack-buffer-emojify t) ;; if you want to enable emoji, default nil
  (setq slack-prefer-current-team t)
  :config
  (slack-register-team
   :name "emacs-slack"
   :default t
   :token "xoxs-sssssssssss-88888888888-hhhhhhhhhhh-jjjjjjjjjj"
   :subscribed-channels '(test-rename rrrrr)
   :full-and-display-names t)

  (slack-register-team
   :name "test"
   :token "xoxs-yyyyyyyyyy-zzzzzzzzzzz-hhhhhhhhhhh-llllllllll"
   :subscribed-channels '(hoge fuga))

  (evil-define-key 'normal slack-info-mode-map
    ",u" 'slack-room-update-messages)
  (evil-define-key 'normal slack-mode-map
    ",c" 'slack-buffer-kill
    ",ra" 'slack-message-add-reaction
    ",rr" 'slack-message-remove-reaction
    ",rs" 'slack-message-show-reaction-users
    ",pl" 'slack-room-pins-list
    ",pa" 'slack-message-pins-add
    ",pr" 'slack-message-pins-remove
    ",mm" 'slack-message-write-another-buffer
    ",me" 'slack-message-edit
    ",md" 'slack-message-delete
    ",u" 'slack-room-update-messages
    ",2" 'slack-message-embed-mention
    ",3" 'slack-message-embed-channel
    "\C-n" 'slack-buffer-goto-next-message
    "\C-p" 'slack-buffer-goto-prev-message)
   (evil-define-key 'normal slack-edit-message-mode-map
    ",k" 'slack-message-cancel-edit
    ",s" 'slack-message-send-from-buffer
    ",2" 'slack-message-embed-mention
    ",3" 'slack-message-embed-channel))

(use-package alert
  :commands (alert)
  :init
  (setq alert-default-style 'notifier))

```
## How to get token

### 1. From Request Payload

 1. Log into the Slack team you're interested in with a web browser
 2. Open DevTools
 3. Open Network tab
 4. Search for (Ctrl-F) "xoxs-" and copy token from Request Payload
 
### 2. From customize page
https://github.com/jackellenberger/emojme#slack-for-web


## How to use

I recommend to chat with slackbot for tutorial using `slack-im-select`.

Some terminology in the `slack-` functions:
- `im`: An IM (instant message) is a direct message between you and exactly one other Slack user.
- `channel`: A channel is a Slack channel which you are a member of
- `group`. Any chat (direct message or channel) which isn't an IM is a group.

- `slack-register-team`
  - set team configuration and create team.
  - :name and :token are required
- `slack-change-current-team`
  - change `slack-current-team` var
- `slack-start`
  - do authorize and initialize
- `slack-ws-close`
  - turn off websocket connection
- `slack-group-select`
  - select group from list
- `slack-im-select`
  - select direct message from list
- `slack-channel-select`
  - select channel from list
- `slack-group-list-update`
  - update group list
- `slack-im-list-update`
  - update direct message list
- `slack-channel-list-update`
  - update channel list
- `slack-message-embed-mention`
  - use to mention to user
- `slack-message-embed-channel`
  - use to mention to channel
- `slack-file-upload`
  - uploads a file
  - the command allows to choose many channels via select loop. In order to finish the loop input an empty string. For helm that's <kbd>C+RET</kbd> or <kbd>M+TET</kbd>. In case of Ivy it's <kbd>C+M+j</kbd>.

## Notification

See [alert](https://github.com/jwiegley/alert).
