## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/Keric28/UnifiedThreat/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Keric28/UnifiedThreat/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.



### Test Pull

$.ajax({
  url: "https://www.bungie.net/platform/GroupV2/" + 379880 + "/Members/",
  headers: {
    "X-API-Key": 074a634dcbb84af181344c647bdc0d18
  }
}).done(function(json) {
  var members = json.Response.results;
  console.log('Member list:', members);
  listMembers(members);
});

function listMembers(rsp) {
  var
  list = $('.memberList-list'),
  sortMembers = function(method) {
    // sort by date joined
    if (method = joined) {
      list.find('.member').sort(function(a, b) {
        return ($(b).data('joined')) < ($(a).data('joined')) ? 1 : -1;
      }).appendTo(list);
    } else if (method = username) {
      list.find('.member').sort(function(a, b) {
        return ($(b).data('username')) < ($(a).data('username')) ? 1 : -1;
      }).appendTo(list);
    }
    list.find('.member.online').prependTo(list);
  };

  for (var i = 0; i < rsp.length; i++) {
    var
    profile = rsp[i].bungieNetUserInfo,
    member = $('<a></a>');
    // check for valid profile
    // some users don't have Bungie profiles somehow and it breaks function
    if (typeof profile != 'undefined') {
      // store response data in semantic variables
      var
      name = rsp[i].destinyUserInfo.displayName,
      joinDate = rsp[i].joinDate,
      joined = joinDate.substring(0, joinDate.indexOf('T')),
      online = rsp[i].isOnline,
      icon = profile.iconPath,
      memberId = profile.membershipId,
      memberType = rsp[i].destinyUserInfo.membershipType,
      destinyId = rsp[i].destinyUserInfo.membershipId,
      rank = rsp[i].memberType;
      // configure DOM node and add to page
      member
      .attr({
        'class': 'j-row vertical-center-row member',
        'href': '/player/?bungieId=' + memberId + '&destinyId=' + destinyId + '&joined=' + joined + '&rank=' + rank,
        'title': 'See player profile for ' + name,
        'data-joined' : joined.replace(/-/g, ''),
        'data-username': name,
        'data-online' : 'false',
        'data-searchable' : name,
      })
      .html(
        '<div class="j-col j-col-1 member-icon"><img src="https://bungie.net/' + icon + '"></div>' +
        '<div class="j-col j-col-3 member-name"><h3>' + name + '</h3></div>' +
        '<div class="j-col j-col-3 member-joined" data-label="Joined">' + joined.replace(/-/g, '/') + '</div>' +
        '<div class="j-col j-col-3 member-status" data-label="Status"><span class="member-online" id="status-' + memberId + '">' + online + '</span></div>' +
        '<div class="j-col j-col-3 member-button"><a class="button outline gold full-width">' + 'View Stats' + '</a></div>'
      )
      .appendTo(list);
      // indicate online/offline status
      if (String(online) === 'true') {
        $('#status-' + memberId)
        .text('Online')
        .addClass('online')
        .closest('.member')
        .attr('data-online', true)
        .addClass('online');
      } else {
        $('#status-' + memberId).text('Offline').removeClass('online');
      }
      sortMembers(joined); // sort members by join date
    }
  }
}
