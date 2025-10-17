<%* tR += (typeof tp.system) %>   <!-- should print 'function'; if 'undefined', it's not exposed -->
- Operator: <% require('os').userInfo().username %>

<%*
const { execSync } = require("child_process");
const gitUser = execSync("git config user.name").toString().trim();
tR += gitUser;
%>