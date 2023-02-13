---
recurringTemplate: true
recurringTemplateName: latest-notes
---

This is where we keep track of our top 10 latest Brainery notes:

<%*
const dv = this.app.plugins.plugins["dataview"].api;
const te = await dv.queryMarkdown(`LIST FROM -"_templates" AND -"_reports" AND -"challenge" WHERE date != NULL SORT date DESC LIMIT 10`);
tR += te.value;
%>

## Newest Contributors
---
<%*
const discordNotes = dv.pages(`-"_templates" AND -"_reports" AND -"challenge"`)
	.where(p => !!p.file.frontmatter.discord_id)
	.where(p => !!p.file.frontmatter.date)
    .where(p => dv.date(p.file.frontmatter.date) !== null)
    .sort(p => p.date, "desc")
	.groupBy(p => p.discord_id)
	.filter(p => p.rows.length <= 1);

for (let group of discordNotes) {
	tR += `- **${group.key}**: `
	for (let row of group.rows) {
		tR += `${row.file.link}\n`
	}
}
%>
---
<%*
const authoredNotes = dv.pages(`-"_templates" AND -"_reports" AND -"challenge"`)
	.where(p => !!p.file.frontmatter.author)
	.where(p => !!p.file.frontmatter.date)
    .where(p => dv.date(p.file.frontmatter.date) !== null)
    .sort(p => p.date, "desc")
	.groupBy(p => p.author)
	.filter(p => p.rows.length === 1);

for (let group of authoredNotes) {
	tR += `- **${group.key}**: `
	for (let row of group.rows) {
		tR += `${row.file.link}\n`
	}
}
%>

*This page was last modified at <%* tR += new Date().toISOString();%>*.