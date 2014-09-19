---
#
# Jekyll template for tablet index web page
#
title: Tablets
layout: page
weight: 10
---
There are several companies (original equipment manufacturers -- OEMs)
designing and making graphics tablet hardware, although origins of their
designs can be traced to just one or two of them. There are many more companies
selling these tablets under their names, i.e. re-branding them.

The tablets below are grouped by their OEMs. A tablet's "original model" name
(if any) is the name used by the OEM or reported by the tablet itself, in its
device descriptor. For the tablets with unknown name, the "original model" is
specified as "PID" followed by the four-digit hexadecimal product ID.

{% assign page_dir = page.url | remove: 'index.html' %}
{% assign sorted_pages = site.pages | sort:'PID' %}

{% comment %}

    Collect vendors

{% endcomment %}
{% assign vendor_list_str = "" %}
{% comment %} For each tablet page {% endcomment %}
{% for p in site.pages %}
    {% assign p_dir = p.url | remove: 'index.html' %}
    {% if p_dir contains page_dir and p_dir != page_dir %}
        {% unless vendor_list_str contains p.vendor %}
            {% capture vendor_list_str %}{{vendor_list_str}} {{p.vendor}}{% endcapture %}
        {% endunless %}
    {% endif %}
{% endfor %}
{% assign vendor_list = vendor_list_str | split:' ' | sort %}


{% comment %}

    For each vendor

{% endcomment %}
{% for vendor in vendor_list %}
{{vendor}}
----------

<table class="tablet_list">
    <thead>
        <tr>
            <th class="original_model">Original model</th>
            <th class="sold_as">Sold as</th>
            <th class="status">Status</th>
        </tr>
    </thead>
    <tbody>
        {% comment %}

            For each vendor's tablet page

        {% endcomment %}
        {% for p in sorted_pages %}
            {% assign p_dir = p.url | remove: 'index.html' %}
            {% if p_dir contains page_dir and p_dir != page_dir %}
                {% if p.vendor == vendor %}
                    {% if p.supported == true %}
                        {% assign status_class = "status-supported" %}
                    {% elsif p.supported == false %}
                        {% assign status_class = "status-unsupported" %}
                    {% else %}
                        {% assign status_class = "status-unknown" %}
                    {% endif %}
                    <tr class="tablet {{status_class}}">
                        <td>
                            {{p.VID}}:{{p.PID}}<br>
                            <a href="{{p_dir}}">
                                {{p.vendor}} {{p.product}}<br>
                                {% unless p.image == null %}
                                    <img src="{{p_dir}}{{p.image}}.thumb.jpg">
                                {% endunless %}
                            </a>
                        </td>
                        <td>
                            <ul>
                                {% for n in p.sold_as %}
                                    <li>{{n}}</li>
                                {% endfor %}
                                {% for n in p.maybe_sold_as %}
                                    <li>{{n}} (?)</li>
                                {% endfor %}
                            </ul>
                        </td>
                        <td>
                            {% if p.supported == true %}
                                Supported
                                {% assign p_supported_in_size = p.supported_in | size %}
                                {% if p_supported_in_size != 0 %}
                                    in:
                                    <ul>
                                        {% unless p.supported_in.kernel == null %}
                                            <li>Linux kernel {{p.supported_in.kernel}}</li>
                                        {% endunless %}
                                        {% unless p.supported_in.wacom == null %}
                                            <li>xf86-input-wacom {{p.supported_in.wacom}}</li>
                                        {% endunless %}
                                    </ul>
                                {% endif %}
                            {% elsif p.supported == false %}
                                Unsupported
                            {% else %}
                                Unknown
                            {% endif %}
                        </td>
                    </tr>
                {% endif %}
            {% endif %}
        {% endfor %}
    </tbody>
</table>
{% endfor %}
